---
publish: true
title: "[x-algorithm] How X Decides What 550 Million Users See"
description: Deep Dive · A code walkthrough of the feed algorithm X open-sourced on January 20, 2026
created: 2026-02-08
modified: 2026-02-09T12:58:46.329+03:00
published: 2026-01-28
tags:
  - api
  - algorithm
  - x
---

### Deep Dive · A code walkthrough of the feed algorithm X open-sourced on January 20, 2026

## Architecture: Component-based Pipeline

The `CandidatePipeline` framework is the modular foundation, defining how recommendation stages interact.

![](https://substackcdn.com/image/fetch/$s_!sYW6!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3ec6c370-4106-427a-b915-4951b03642ce_734x486.png)

Key component types are

- **Sources** fetch candidate posts. _Thunder_ retrieves posts from accounts you follow (in-network). _Phoenix_ serves as a general discovery engine, using ML similarity matching to find relevant posts across the entire platform (both out-of-network and in-network).
- **Hydrators** enrich candidates with additional data, like whether a post is in-network, engagement statistics, author metadata.
- **Scorers** and **Filters** evaluate and prune the enriched candidate pool.

### Pipeline Execution Model

The `CandidatePipeline` trait defines the pipeline interface. Concrete implementations like \`PhoenixCandidatePipeline\` wire together the specific components.

[candidate\_pipeline.rs#L36-L92](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/candidate-pipeline/candidate_pipeline.rs#L36-L92)

![](https://substackcdn.com/image/fetch/$s_!B3Ag!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F072a5188-8243-4b0d-8a2f-bbfd42b69198_1080x2456.png)

Every timeline request triggers the full pipeline. When you open “For You,” the server executes the entire pipeline.

[server.rs#L24-L83](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/home-mixer/server.rs#L24-L83)

![](https://substackcdn.com/image/fetch/$s_!LCmA!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F03fb7300-158c-4541-a8eb-eab388e903cb_1080x1488.png)

The **Phoenix Scorer** uses a Grok-based transformer to predict engagement. It generates a unique request ID and timestamp for each call:

[phoenix\_scorer.rs#L19-L26](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/home-mixer/scorers/phoenix_scorer.rs#L19-L26)

![](https://substackcdn.com/image/fetch/$s_!ipSF!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F2e7d3382-b2a7-4411-9fbe-055ba6885d2f_1080x594.png)

This means a post’s score can change between requests as engagement accumulates. For example, if User A sees your post at hour 1 (100 likes), and User B sees it at hour 3 (500 likes), Phoenix receives different engagement features and may produce different predictions.

## Stage 1: Finding Candidates

The primary goal of retrieval is to funnel the massive corpus of millions of posts down to a manageable set of thousands of candidates for ranking.

In-network posts come from your follows. Out-of-network (OON) posts are discoveries based on your interests. In-network content keeps its full score, but OON content is penalized during ranking.

Two sources feed the pipeline: Thunder and Phoenix.

![](https://substackcdn.com/image/fetch/$s_!Pk-p!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd03869cb-d286-4f53-bb62-151671f84efc_1242x692.png)

### Thunder: In-Network Content

Thunder is a separate service maintaining an in-memory `DashMap` (concurrent HashMap) of the last [48 hours](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/thunder/posts/post_store.rs#L524) of tweets, indexed by author. `home-mixer` queries Thunder via gRPC:

[thunder\_source.rs#L12-L74](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/home-mixer/sources/thunder_source.rs#L12-L74)

![](https://substackcdn.com/image/fetch/$s_!eHwG!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8e2e32b8-5eb1-45b8-b8e5-7f359e35f323_1080x1750.png)

Thunder’s `PostStore` maintains indices for original posts, replies/retweets, and video content, enabling O(1) lookups.

[post\_store.rs#L36-L53](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/thunder/posts/post_store.rs#L36-L53)

![](https://substackcdn.com/image/fetch/$s_!mLBp!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F09dc8385-062c-4370-8dcb-4384e8c47d5f_1080x892.png)

### Phoenix: Discovery

Phoenix uses a two-tower neural network for retrieval.

![](https://substackcdn.com/image/fetch/$s_!6pKu!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fbf675b93-d78e-43e9-bdb0-9eacda3c4142_1150x926.png)

#### User Tower: Transformer + Mean Pooling

The user tower feeds user features and engagement history through the Grok transformer, then mean-pools the output.

[recsys\_retrieval\_model.py#L206-L276](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/phoenix/recsys_retrieval_model.py#L206-L276)

![](https://substackcdn.com/image/fetch/$s_!y7z8!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F07ed2cc3-2bee-44a2-bd67-c7c5d6e4e9c1_1080x1786.png)

#### Candidate Tower: A 2-Layer MLP

The candidate tower is simple: a 2-layer MLP with SiLU activation projecting post+author embeddings into the shared space.

[recsys\_retrieval\_model.py#L47-L99](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/phoenix/recsys_retrieval_model.py#L47-L99)

![](https://substackcdn.com/image/fetch/$s_!vhYD!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6c694805-617b-4beb-95fd-712e963a6e9c_1080x2308.png)

#### Retrieval: Dot Product Over Corpus

With L2-normalized embeddings, retrieval is a matrix multiplication:

[recsys\_retrieval\_model.py#L346-L372](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/phoenix/recsys_retrieval_model.py#L346-L372)

![](https://substackcdn.com/image/fetch/$s_!03u3!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F39262b7f-e617-4004-80dd-06a6fdc12a16_1080x1004.png)

At request time, Phoenix generates a single user embedding to query millions of precomputed candidates. Top-k selection uses dot product similarity, likely optimized via Approximate Nearest Neighbor (ANN) to avoid the latency of a brute-force search.

Thunder and Phoenix return a pool of candidate posts, each as a `PostCandidate` struct. Classification happens next.

## Stage 2: Classifying In-Network vs. Out-of-Network

Before scoring, the `InNetworkCandidateHydrator` determines if a post originates from a followed account.

[in\_network\_candidate\_hydrator.rs#L10-L39](https://github.com/xai-org/x-algorithm/blob/aaa167b/home-mixer/candidate_hydrators/in_network_candidate_hydrator.rs#L10-L39)

![](https://substackcdn.com/image/fetch/$s_!-LGY!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F77d44108-7a4b-46ea-900c-ab25e9c98297_1080x1376.png)

It converts followed accounts to a `HashSet` for O(1) lookups. Your own posts (`is_self`) are also in-network.

Why use a classifier? Phoenix (discovery) can sometimes return posts from followed accounts. The hydrator ensures that any post from a followed account gets in-network status, regardless of whether it came from Thunder (subscription) or Phoenix (discovery).

The boolean flag `in_network` propagates through the pipeline, affecting scoring and filtering. Separating classification from scoring allows independent evolution.

## Stage 3: Pre-Filtering

Ten sequential filters prune the candidate pool before scoring to ensure quality and save computation.

- DropDuplicatesFilter: Dedupes posts appearing in both Thunder and Phoenix.
- CoreDataHydrationFilter: Drops incomplete candidates (missing ID/text).
- AgeFilter: Enforces `MAX_POST_AGE` limits.
- SelfTweetFilter: Excludes your own posts.
- RetweetDeduplicationFilter: Resolves original vs. retweet duplications.
- IneligibleSubscriptionFilter: Hides locked content from unsubscribed authors.
- PreviouslySeenPostsFilter: Skips viewed posts (Bloom filters/IDs).
- PreviouslyServedPostsFilter: Skips posts served in the current session.
- MutedKeywordFilter: Enforces keyword mutes.
- AuthorSocialgraphFilter: Enforces block and mute lists.

[phoenix\_candidate\_pipeline.rs#L108-L120](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/home-mixer/candidate_pipeline/phoenix_candidate_pipeline.rs#L108-L120)

![](https://substackcdn.com/image/fetch/$s_!pfpT!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0bfd6c4d-5ff9-408a-9693-e88fbcdd8130_1080x706.png)

Only unique, fresh, eligible, and safe candidates proceed to scoring.

## Stage 4: Scoring

This is where posts compete. Scoring happens in four sequential steps, defined in the pipeline configuration:

[phoenix\_candidate\_pipeline.rs#L122-L132](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/home-mixer/candidate_pipeline/phoenix_candidate_pipeline.rs#L122-L132)

![](https://substackcdn.com/image/fetch/$s_!E05G!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc5605f2f-5320-49db-bda1-aa2d95945c27_1080x670.png)

The order matters because each scorer depends on the previous output.

![](https://substackcdn.com/image/fetch/$s_!lClC!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F093ae8e0-9dff-48b7-a8a1-62f84602c62c_1414x322.png)

### Step 4.1: Phoenix Transformer Predictions

Phoenix predicts 18 engagement probabilities and one continuous metric for each post.

Positive signals:

- `favorite_score`: Probability you’ll like
- `reply_score`: Probability you’ll reply
- `retweet_score`: Probability you’ll repost
- `photo_expand_score`: Probability you’ll expand a photo
- `click_score`: Probability you’ll click
- `profile_click_score`: Probability you’ll visit author’s profile
- `vqv_score`: Video quality view (watching to completion)
- `share_score`, `share_via_dm_score`, `share_via_copy_link_score`: Sharing probabilities
- `dwell_score`: Probability you’ll stop scrolling
- `quote_score`: Probability you’ll quote tweet
- `quoted_click_score`: Probability you’ll click on a quoted tweet
- `follow_author_score`: Probability you’ll follow after seeing

Negative signals:

- `not_interested_score`: Probability you’ll click “Not interested”
- `block_author_score`: Probability you’ll block
- `mute_author_score`: Probability you’ll mute
- `report_score`: Probability you’ll report

Continuous metric:

- `dwell_time`: Predicted time you’ll spend viewing the post

Phoenix scores all candidates in a single forward pass using a custom attention mask. But how do you batch candidates without their scores affecting each other?

#### Independent Scoring (Batch Independence)

Standard attention would allow candidates in the same batch to influence each other’s scores. To ensure a post’s score depends _only_ on the user context, X uses `make_recsys_attn_mask` to isolate candidates.

[grok.py#L39-L71](https://github.com/xai-org/x-algorithm/blob/aaa167b3de8a674587c53545a43c90eaad360010/phoenix/grok.py#L39-L71)

![](https://substackcdn.com/image/fetch/$s_!C_8b!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F6be1ac43-23f3-42fd-8ce5-dd1b63a69545_1080x1414.png)

Each candidate attends to the user context and itself, but interaction with other candidates is blocked.

![](https://substackcdn.com/image/fetch/$s_!IN-M!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd2d18438-1b25-4fbf-8df2-90b7da3e3b8e_1056x596.png)

Each candidate gets the same score regardless of batch composition.

### Step 4.2: Weighted Scoring

`WeightedScorer` combines the 18 probabilities and one metric into a single number:

[weighted\_scorer.rs#L44-L70](https://github.com/xai-org/x-algorithm/blob/aaa167b/home-mixer/scorers/weighted_scorer.rs#L44-L70)

![](https://substackcdn.com/image/fetch/$s_!kwMI!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ffc046c07-afe4-44a4-9d2f-55581ef3892c_1080x1600.png)

The last four weights are negative. Content that predicts blocking is penalized.

### Step 4.3: Author Diversity—The Decay Function

To prevent one author from dominating your feed, X applies exponential decay.

![](https://substackcdn.com/image/fetch/$s_!hX8f!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F79ac5cde-6c73-4fa3-b6ee-62bd0f96c406_1080x2494.png)

The decay curve is

![](https://substackcdn.com/image/fetch/$s_!j8Q8!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d3b34a9-b855-462d-b107-22a09a9c2bd7_1561x880.png)

The floor parameter (e.g., 0.3) ensures even a prolific author’s later posts retain some score, balancing diversity with relevance.

### Step 4.4: Out-of-Network Penalty

Finally, `OONScorer` penalizes posts from accounts you don’t follow.

[oon\_scorer.rs#L10-L33](https://github.com/xai-org/x-algorithm/blob/aaa167b/home-mixer/scorers/oon_scorer.rs#L10-L33)

![](https://substackcdn.com/image/fetch/$s_!Q1Ry!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F62547e9e-3785-4586-be1d-1991a280b9c0_1080x1228.png)

Posts from non-followed accounts are multiplied by `OON_WEIGHT_FACTOR` (e.g., 0.7), making them harder to rank high unless they are engaging.

## Stage 5: Selection and Final Filtering

The `TopKScoreSelector` picks the top K candidates by score.

[top\_k\_score\_selector.rs#L6-L15](https://github.com/xai-org/x-algorithm/blob/aaa167b/home-mixer/selectors/top_k_score_selector.rs#L6-L15)

![](https://substackcdn.com/image/fetch/$s_!_Mhw!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F38a59661-a402-42e1-b389-e629b071442f_1080x558.png)

Results are sorted and truncated to `size()`. Before returning, post-selection hydration and filtering clean up the top set.

### Visibility Filtering

Before the filter runs, `VFCandidateHydrator` queries an external visibility service. It applies different safety levels: `TimelineHome` for in-network posts, and the stricter `TimelineHomeRecommendations` for out-of-network posts. Both calls run in parallel.

`VFFilter` then drops any flagged candidate.

[vf\_filter.rs#L7-L33](https://github.com/xai-org/x-algorithm/blob/aaa167b/home-mixer/filters/vf_filter.rs#L7-L33)

![](https://substackcdn.com/image/fetch/$s_!TK2L!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F7e38e873-7363-430a-829f-9cfea4d1b0d6_1080x1004.png)

Any FilteredReason or `Action::Drop` triggers removal, leaving only posts with no visibility issues. The specific safety labels, like spam or policy violations, are determined by the external xai\_visibility\_filtering service.

### Conversation Deduplication

`DedupConversationFilter` keeps only the highest-scored post per conversation thread:

[dedup\_conversation\_filter.rs#L8-L51](https://github.com/xai-org/x-algorithm/blob/aaa167b/home-mixer/filters/dedup_conversation_filter.rs#L8-L51)

![](https://substackcdn.com/image/fetch/$s_!Ai7k!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0feffe0f-1707-4b61-b887-4210cdbff902_1080x1750.png)

The conversation ID is the minimum ancestor ID, the root of the thread. If multiple posts from the same thread reach the top set, only the highest-scored survives. Standalone posts (no ancestors) use their own `tweet_id`, so they always pass.

## The Complete Data Flow

![](https://substackcdn.com/image/fetch/$s_!7Q3C!,w_424,c_limit,f_webp,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d3aaba1-f38c-43c1-8ac1-1b8a5f32ae9e_898x1250.png)

## What This Means for Content Creators

The formula reveals what the algorithm values:

1. Maximize Positive Engagement: Likes, replies, shares, and video completion increase your score.
2. Minimize Negative Engagement: Blocks, mutes, and reports heavily penalize you.
3. Quality > Quantity: Exponential decay limits the reach of spamming.

### Why does controversial content persist despite penalties?

**Volume overwhelms penalty.** A post with 10,000 likes and 100 blocks might score: `10,000×30 - 100×100 = 290,000` (using hypothetical weights). The sheer volume of positive engagement drowns out the penalty. Rage bait works when the engaged audience vastly outnumbers the offended minority.

**User segmentation matters.** Phoenix predicts per-user probabilities. If _you_ historically engage with controversial content without blocking, the model predicts low `block_author_score` for _you_ specifically. Rage bait isn’t shown to everyone, but it’s selectively served to users who tolerate it.

**Quote-tweets count as positive.** Angry quote-tweets trigger `quote_score`, which has a positive weight. Outrage sharing is still sharing in the algorithm’s eyes. The model can’t distinguish “quoting to criticize” from “quoting to endorse.”

**Timing asymmetry.** Early positive engagement triggers distribution. By the time blocks and reports accumulate, the post has already reached millions. The algorithm reacts to signals; it doesn’t predict future backlash.

### Does the first hour matter?

There is no velocity scoring. No multiplier for “fast” likes.

- Thunder (In-Network) fetches last 48h, sorted new-to-old. Recency is baked in.
- Phoenix retrieve posts semantically. Older viral posts _can_ surface if they match your interests, up to `AgeFilter` limits.

Early engagement still matters because of feedback loops.

- Fresh Data: Phoenix re-scores with current stats on every request.
- Compounding: More engagement → higher probability → more distribution → more engagement.
