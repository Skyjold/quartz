---
publish: true
title: "#982 The Third Dice - Project Euler"
description: A website dedicated to the fascinating world of mathematics and programming
created: 2026-02-03
modified: 2026-02-06T19:14:31.622+03:00
published: 2026-02-06T19:14:31.622+03:00
tags:
  - clippings
  - "#eurler"
  - "#promlem982"
---

![projecteuler.net](https://projecteuler.net/images/clipart/print_page_logo.png)

## The Third Dice

### Problem 982

Alice and Bob play the following game with two six-sided dice (numbered to ):

1. Alice rolls both dice; she can see the rolled values but Bob cannot
2. Alice chooses one of the dice and reveals it to Bob
3. Bob chooses one of the dice: either the one he can see, or the one he cannot
4. Alice pays Bob the value shown on Bob's chosen dice

Each player devises a (possibly non-deterministic) **strategy**. An example strategy for each player could be:

- Alice chooses to reveal the dice with value closest to , or if both are equidistant she chooses randomly with equal probability
- Bob chooses the revealed dice if its value is at least ; otherwise he chooses the hidden dice

In fact, these two strategies together form a **Nash equilibrium**. That is, given that Bob is using his strategy, Alice's strategy **minimises** the expected payment; and given that Alice is using her strategy, Bob's strategy **maximises** the expected payment.

With these strategies the expected payment from Alice to Bob is .

To make the game more interesting, they introduce a third (six-sided) dice:

1. Alice rolls **three** dice; she can see the rolled values but Bob cannot
2. Alice chooses **two** of the dice and reveals both to Bob
3. Bob chooses one of the three dice: either one of the two visible dice, or the one hidden dice
4. Alice pays Bob the value shown on Bob's chosen dice

Supposing they settle on a pair of strategies that form a Nash equilibrium, find the expected payment from Alice to Bob, and give your answer rounded to six digits after the decimal point.
türkçeleştirme ;

Alice ve Bob, **iki adet altı yüzlü zar** (1’den 6’ya numaralandırılmış) ile aşağıdaki oyunu oynar:

1. Alice iki zarı atar; gelen değerleri **görür**, Bob **göremez**.
2. Alice zarların **birini seçer** ve Bob’a **gösterir**.
3. Bob bir zar seçer: **gördüğü zarı** ya da **gizli olan zarı**.
4. Alice, Bob’un seçtiği zarın **üzerindeki sayı kadar** Bob’a ödeme yapar.

Her iki oyuncu da (rastgelelik içerebilen) birer **strateji** belirler. Örnek stratejiler:

- Alice, değeri **3.5’e en yakın** olan zarı gösterir; eğer iki zar eşit uzaklıktaysa **rastgele** seçer.
- Bob, gösterilen zarın değeri **en az 4 ise** onu seçer; değilse gizli zarı seçer.

Bu iki strateji birlikte bir **Nash dengesi** oluşturur. Yani:

- Bob bu stratejiyi kullanıyorken Alice’in stratejisi **beklenen ödemeyi minimize eder**,
- Alice bu stratejiyi kullanıyorken Bob’un stratejisi **beklenen ödemeyi maksimize eder**.

Bu stratejiler altında Alice’in Bob’a yaptığı **beklenen ödeme 3.5’tir**.

---

Oyunu daha ilginç hâle getirmek için **üçüncü bir altı yüzlü zar** eklenir:

1. Alice **üç zar** atar; değerleri görür, Bob görmez.
2. Alice zarların **ikisini seçer** ve Bob’a **gösterir**.
3. Bob üç zardan birini seçer:
   - Gösterilen iki zardan biri
   - Ya da **gizli** olan zar
4. Alice, Bob’un seçtiği zarın **değeri kadar ödeme yapar**.
   Alice ve Bob’un **Nash dengesi oluşturan** stratejiler üzerinde anlaştığını varsayalım.
   Bu durumda Alice’in Bob’a yapacağı **beklenen ödeme** nedir?
   Cevabınızı **virgülden sonra 6 basamak** olacak şekilde yuvarlayarak veriniz.
