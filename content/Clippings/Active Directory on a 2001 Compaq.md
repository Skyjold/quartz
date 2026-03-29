---
publish: true
title: Active Directory on a 2001 Compaq
description: Sevdiğiniz videoların ve müziklerin keyfini çıkarın, orijinal içerik yükleyin ve tümünü YouTube'da arkadaşlarınızla, ailenizle ve dünyayla paylaşın.
created: 2026-03-28
modified: 2026-03-28T10:01:48.181+03:00
published: 2026-03-28
tags:
  - clippings
---

![[bd13745932b0a53a40e35065bc75efcf_MD5.unknown]]

Let's get Windows 2000 Advanced Server installed on a Compaq DL380 Gen 2 from 2001. We'll go over all the hardware, learn about how the remote management board works, and get this thing racked up in the Retro Rack. Finally we'll install Active Directory and remotely administer some other Windows machines.

Check me out on Patreon: https://www.patreon.com/clabretro

Music by Karl Casey @ White Bat Audio

#netro #retrotech #retrocomputer #networking #homelab #cisco #telephony

Serial to USB converter: https://amzn.to/4mAXDlJ

Rack stuff\
StarTech Universal Rack Rails: https://amzn.to/404JV1Q\
StarTech 42U Rack: https://amzn.to/482c3oh\
StarTech 25U Rack: https://amzn.to/3mEB7hS\
Tripp Lite SMART1500LCD UPS: https://amzn.to/3KZW3Jw\
1U 24 Port Patch Panel: https://amzn.to/3Nm0bFa\
1U Brush Panel: https://amzn.to/3mExAA3\
1U Rack Shelf: https://amzn.to/3oaDclT

Video gear\
Camera: https://amzn.to/4al3xjA\
Mics: https://amzn.to/4dCUuO2

Note: The above are Amazon affiliate links. It doesn't cost you extra, but I'll receive a commission which will help keep the content coming. I only link to things I've personally ordered.

00:00:00 Intro\
00:02:24 DL380 Gen 2 Physical Overview\
00:08:33 Inside the Server\
00:15:39 Remote Management Cards\
00:21:40 Lid Diagram\
00:26:53 First Power On\
00:31:32 Using Remote Management\
00:42:09 Racking it Up\
00:51:24 KVM Setup\
00:56:28 Attempting Windows 2000 From CD\
01:04:40 SmartStart\
01:15:23 Trying Storage Drivers from Floppy\
01:25:06 Slipstreaming Drivers\
01:30:29 Installing Active Directory\
01:37:03 Setting up DNS\
01:40:45 Controlling Another Computer\
01:53:17 Outro

## Transcript

### Intro

**0:00** · This is the retro rack. It's where I host mid90s to mid2000s enterprise networking and server gear. One of my goals is to use this in a somewhat realistic fashion, as you would have back in the day with all this gear. For example, one of my Sun Microsystem servers is hosting some Sunray server software that lets me hook up Sunray Thin clients. And one of the Cisco units is hosting a full VoIP setup with a bunch of Cisco IP phones around the basement. From a networking perspective, you can see we're pretty much a Cisco shop. And from a server and software perspective, we're pretty much all Unix.

**0:31** · We've got two of these Sun Micro Systemystems Sunfires running Solaris.

**0:35** · And then down at the bottom, we've got a pile of IBM System P series gear running IBM's AIX. There's still a couple spots open in the rack, though. And the boss came to me today and said, "We got to get in on this Windows thing." And this is how we're going to do that. This is a compact Prolantiant DL380 Gen 2. And I think it's sporting a couple Intel Pennium 3es inside. I believe this came out in 2001, just before HP acquired Compact. So, of course, it's got all the Compact branding on there still. I think HP left that around for quite a while, though. In fact, HPE will still sell you a Prolant DL380. I think they're on Gen 11 or Gen 12 now. Best I can tell, these were pretty well regarded. In a Network World review from August of 2001, it got really good marks, which is encouraging because it would have set you back around $9,000 list price. Now, there were probably people running Windows NT4 on this thing, but I think more appropriate would be Windows 2000, and my experience with DL380s and Windows NT hasn't been exactly fun. So, we're going to get Windows 2000 Server on this thing. Maybe maybe we'll be able to use that product key. Then we're going to try to get this thing working as an active directory server so we can use other computers. Maybe like this compact laptop here running Windows XP. And I'll be able to manage the users on my network from the active directory. Maybe even some file stuff. We'll find out. I also believe this thing has a remote insight lights out edition card in it, which means some level of remote management 2001 style. So let's open this thing up. See what's going on. See if we can get Windows 2000 on it. fire up Active Directory. Probably do a bunch of cabling and KVM funny business.

**2:16** · Should be fun. Let's get into it.

### DL380 Gen 2 Physical Overview

**2:24** · A viewer named Matt sent this my way for the cost of shipping. In fact, there was another one.

**2:30** · Yesesh.

**2:32** · This is a slightly newer uh DL380 G5 Gen 5. It's got the HP branding and a front diagnostics panel straight out of the USS Enterprise. It's also got a CDROM, DVD ROM, maybe. The Gen 2 that we're concerning ourselves with today does not have a CDROM installed, but I'm hoping we can install it with the remote card that I'll show you in a little bit. And shipping was uh 150 bucks or something.

**3:00** · The stickers on the side are claiming upwards of 60 lb or 27 kg. And I don't really buy that. I think these have drives in them. Anyway, it feels a lot like a lot more than 60 lbs when you're carrying these things around. But the G5 here came with rails. I've got the rest of the rail kit up in the garage. And I don't have rails for the one we want to rack. So later in the video, we'll see.

**3:22** · Maybe these are compatible the way server manufacturers like to do their stuff. I'm guessing they're not, but we'll find out. Can't remember what year this Gen 5 came out, but uh it's a pretty cool looking unit. Uh, if you have any ideas for what I should do with this one, let me know. Windows Server 2003. I don't know. Oh my god. On the front, we've got six drive bays. These are Ultra 3 Scuzy. And I think maybe this was being used in a home lab capacity at one point in its life because they all have drives. And I think I was reading one of these bays can be used for a tape drive, which is pretty cool. Looks like all six bays were populated and they are all 73 gig 15k RPM Ultra 3 SCSI drives. The three in the bottom were all the same. Seagate cheetah drives. These are good in my experience though. I'm only just messing around down here, but don't think I've seen a Cheetah drive fail on me yet. And then the top drives were all Maxtor drives. Atlas 15,000 RPM 73 gig Ultra 320 SCSI. I guess I'll leave them out for now and maybe I'll try to keep track of where they were. I don't think there's an OS on this thing, but I will make a little system for myself to remember which ones went where. I think Matt was telling me a couple of them are unhappy.

**4:47** · The LED indicators are indicating that a couple of these drives are not doing too hot. On the front, a power button. Of course, we have an empty slot for a CDROM. And there is a floppy drive in here. You'll see that when we take the lid off. And even without the drives, it's it's still pretty heavy. Around back, some redundant hot swappable power supplies. What do we got here? They can take 100 to 240 volts in AC. They output positive 12 at 32 amps. Okay. - 12.3 amps. Plus 5 ox. It says at 5 amps. I always still get excited when I see compact branded gear.

**5:28** · I don't know why. It's cuz I don't know.

**5:29** · They're a part of computing history and they're gone.

**5:33** · Pretty cool. Pretty compact, easy to pull out and put in as far as power supplies go. And then you get a really nice funny spring noise as you pop them back in. Nothing too surprising back here except we do have two network interfaces builtin on board. I think the Gen One only had one of those. serial port, a VGA, we got two USB, I don't know what speeds, and then probably slow. And then PS2, mouse and keyboard, and then there's this UID uh light and button. I think it stands for unit identifier, something like that. I can't remember. I saw it in the manual briefly, but it's a button, and sometimes you can kind of get it to come out. Now, now I think it's in the out position. Basically, this is a light you can turn on probably with the remote management software to let someone working in the rack, probably yourself as you walk back there, know which computer you want to pull the power supply out of or what whatever service you're doing. And my guess is that you can press this and get it to turn on.

**6:32** · There's probably a complimentary indicator on the front. Over here, we've got a scuzzy connector probably for like a disc shelf or something. And then three PCI, don't quote me on that. PCI or PC X maybe. I'll put it on the screen after I read the documentation. One operating at 33 MHz and then two of them at 66 MHz. And I think I was reading the top two are hot swappable. So if you've got a card that goes bad, you can swap it out. And I'm guessing that's what these indicators for the top two are here. Though I don't quite understand what they mean. So it's labeled two and three. Each of them has an LED. And then in here is maybe the most exciting part of this particular machine. This is a remote management card. Compact had a crazy name for it. Remote insight lights out edition card. You can get network.

**7:21** · This funny little D connector thing is for a mouse and keyboard and then VGA.

**7:26** · And then you would power it with a barrel jack power supply. So this card remains independently powered from the machine. And you can use this card to remotely, you know, boot it up. We might be able to install the OS with this without a CDROM. We'll explore that.

**7:43** · Later versions had ISO integrated lights out management which would have been just installed directly on the main board or somewhere in the chassis and you didn't have to buy this extra card.

**7:52** · I have one of these cards that I've tested in a Gen One actually in the compact box. Racking up 20 of these on new server day or whatever would have been so fun unboxing everything. So, here it is. Most importantly, that funny connector in there, I've got the cable.

**8:11** · So, we should be able to fully use this card. It's pretty weird. PS2 mouse and keyboard that you can probably plug in to like a KVM or it lets you plug a mouse and keyboard in directly with this ridiculous T dongle. Here's the one I have. It'll be interesting to see if it's identical to the one in this Gen 2.

**8:28** · I, like I was saying, I have successfully used this on a Gen 1 DL380.

### Inside the Server

**8:33** · Let's get inside. This is unnecessarily complex in my opinion. You flip this up.

**8:39** · You twist that to release a little latch.

**8:42** · And then you can theoretically you can take the case off.

**8:48** · There we go.

**8:52** · Oh yeah. In the late 90s into the early 2000s, as the industry, in my opinion, was sort of standardizing on what a one U and a 2U server looked like, there was some clever stuff going on. And they're all a little different between the manufacturers. Uh Sun especially was doing off doing their own thing. But I must say, for being a machine from 2001, this looks like an awfully lot like a server you would pop open today. Pretty cool. I didn't actually look on the other side of this. Are we going to get a sweet diagram?

**9:22** · Oh yeah, better believe it. We'll look at that in more detail later, but first we got to look at this area. This section is dedicated to this Torx T15 service tool, which is still here. And it's kind of interesting. It says you do not need this tool to install compact server options. They tried to make this thing very toolless. And then down in here, it says remote insights lights out edition interface cable. If we're going to install the remote insights lights edition expansion board, use this cable stored in the area adjacent to the tape drive bay. I told you one of these was a tape drive bay. It's got this little plastic bag, which of course is empty cuz our I'm just going to call it the remote management card. Our our card is installed. This is very interesting to me though because mine came with that cable. I don't think I ever used it.

**10:17** · Usually I have it in this bag, but we're going to be looking at it again later.

**10:20** · Yeah, here it is. Pretty sure this is the same thing or the same idea. Anyway, so the card we have must have this installed for whatever reason. We'll get into that. Here's the CDROM drive slot.

**10:34** · And it's got what is clearly a an impressive ejection mechanism. It looks like I should be able to push a button on the front. see that kind of moving around. This black piece of plastic is clearly supposed to touch this eject lever. But when I press on the front, it doesn't move any further than that.

**10:53** · Maybe you can just barely see. There's this little slot connected to that black piece of plastic. Here's where the CDROM drive would go. So, yeah, it's some some sort of ejection mechanism because I can I can press it myself.

**11:08** · Pull out the blanking plate. Very high quality, impressive blanking plate. I guess this would have been to stop air flow. That's very important in these machines. There's the board the drive would plug into. It is compact branded and it's got a year 2000 copyright on it. Got a very slim floppy drive connected to that and it is plugged into this sort of back plane board that all the drives slot into. And this is the little piece of metal that the case mechanism latches onto. Moving further in, we have an impressive array of six what I can only imagine are very loud fans. And with these enterprise servers, typically a a reddish or orish color means you can do it while the machine's running. And a bluish color means whatever you're messing with has to happen when the machine is off. So these can be replaced while the machine is running.

**12:06** · Back in here, more removable fans. And then these uh appear to be dual pennium 3es.

**12:17** · I'm not going to mess with it.

**12:19** · Oh, and have a clock battery. I wonder if I got one of those. Yeah, let's pull her out.

**12:26** · Is it going to come easy?

**12:28** · No. I'll be Give me Give me a second. A big dog. 3volt lithium ranata CR 2450N.

**12:38** · I might actually have one of these sitting around. Let me go look. I do exactly one of them. I think the N denotes that kind of taller portion.

**12:47** · These are 2450s.

**12:50** · They don't have that. The 2450 end kind of has that notch in it. But anyway, this thing's totally dead.

**12:58** · 36 molts. 40 molts. kind of moves around.

**13:03** · The replacement is the same brand and everything. Still made by the Swiss.

**13:07** · Might as well pop that in here while we're around. Over here is going to be the power regulation for the CPUs, I'm guessing. Really hope we have both.

**13:19** · I don't see why they'd put a heat sink on if there wasn't a chip under there.

**13:23** · So, that's exciting. Let's see if I can get this back on. Heat sink removal and disposal. I don't think I've ever seen this before. It says if the heat sink is removed, it must be replaced with a new heat sink. That doesn't sound accurate, but what do I know?

**13:39** · And then down in here, we've got the RAM. Got some compact part number stickers on there. So, maybe the original stuff that came with the unit.

**13:46** · Pop one out. I think it's PC133.

**13:50** · Take a look here. Yeah, PC133 512 megabytes ECC. So, we got some server RAM, of course, and we've got four of them. So, this thing has 2 gigs of RAM.

**14:03** · Not bad for 2001, I guess. And then, finally, the moment I've been waiting for. Let's take a look at that remote management card. Check this out. So, what am I doing here? You can remove this entire cage. And of course, it's blue. So, it's telling you you better unplug the machine first before you do it. Whole assembly comes out. And then over here, get this uh dust out of the way. So, here you can unlock.

**14:34** · And these have a red color, which means you can do this live. I don't really understand what this is. This must help you pull the card out. Let me go find a card.

**14:46** · Here's one. A rather impressive one. I think this is a raid card of some sort.

**14:53** · I'm guessing it's not compact because they like to clearly mark their stuff, but it is a six 66 MHz PCI card.

**15:05** · Oh man, this would be so nerve-wracking doing this if this machine was running. Is this just bare metal on the top there?

**15:16** · We're in.

**15:19** · We saved the day in 2001 because we needed to replace the RAID card. That would not save anyone's day. So, what's the story? What is this thing?

**15:29** · It does nothing.

**15:34** · Man, imagine pulling this out while this machine is running. Oh my gosh. I think we need to take this whole thing out to get the bottom card out, the remote management card, because it's not accessible past the case here. Hence, only two hot swappables, I suppose. And I think that's our little mystery remote insight lights out edition interface cable down in there. So, theoretically, we loosen these.

### Remote Management Cards

**16:03** · Maybe we'll use our Torx bit here.

**16:09** · Do the other one.

**16:12** · Toolless my ass.

**16:15** · Oh man.

**16:18** · Drop the Torx bit. Oh yeah. It's It all comes out. Whoa. Wow. So here's the bottom of that card and here's that cable. And it is a tight fit. It goes right from the card onto the riser board itself. I feel like I'm going to regret doing any of this. And to get the card out, just in case you know you forgot to turn the machine off and you're this far.

**16:43** · They put the blue screw on there.

**16:46** · I think you're pretty cooked. The machine's running at this point.

**16:50** · And I dropped the screw. Like I was saying, Compact put their name everywhere and I never get tired of seeing it. Let's take a look at this thing. See how similar or dissimilar it is to the one I already have. It's looking pretty similar. Taking a look at these, I'm pretty sure they are the same board, as in they're the same part number that you would order right down to the cap-on tape. They had to put there on my board and in the same exact position on the one I just pulled out.

**17:17** · There are some differences though, and because it interests me, we will be looking at them in excruciating detail.

**17:23** · They've got this little like part number tag area, and this SP number is the same for both. Same with this AS number. This number here is different. This is the board I just pulled out. It's a Rev0E.

**17:39** · Even though over here it says Rev0 F.

**17:42** · And then the one I already have is a Rev0A.

**17:46** · It says RD over here. Mine says assembled in USA. The one I just pulled out says product of US contains foreign content.

**17:57** · Very scary. So I would say basically they're the same. Mine's just older.

**18:01** · They both have this remote insight board header, though. I don't know if that label corresponds to this header. But at any rate, someone's been in here and uh tweaked the pin on the one I just pulled out of the machine. Mine is pristine cuz I think it was new in box. They both have this little switch header thing.

**18:21** · All the switches are in the same position. And this far switch here has obviously been messed with. The plastic is messed up. Mine is exactly the same.

**18:30** · So, let me pull mine over here for you.

**18:33** · Also, someone got in there. So, maybe during factory testing, it's in one position and when they ship it out, they get in there and they jam the switch a different direction. I don't know. They both have a copyright compact 1999 to 2000 right in here. And I wouldn't say I'm like an idiot or anything, but I'm I'm no hardware designer. And so the amount of engineering that goes into just these random boards that we don't think twice about absolutely fascinates me. Like was it one guy that built this? Was it a team of 300 people at Compact that built this? How long did it take? How long were these relevant? Because we're two generations in at least. Were they an iteration of an older variant before they started doing the Proliance? Like I have no idea. But this stuff is just fascinating. Like these are non-trivial boards as you'll see. Hopefully my hopefully this one works. The good news is if this one doesn't work, though I suspect it does. This one will for sure because I know it does. Yeah, I'm going to put this all back together in the riser cage. But before we do that, we'll just marvel at the size of an Enterprise mainboard. They are typically bespoke and it's just incredible. Also, I think this is the CPU power regulations. So, I don't know what's going on back here.

**19:54** · I'm going to flip the video for you, but it's a system board compact 2000 to 2001 copyright. There's also this rather impressive looking board doted onto it with a chip made in Canada. It's a little power PC hiding in here. Kind of looks like a power PC. It's kind of hard to show, but it's uh for home and office use, whatever it is. The FCC was involved. Here's that PCI riser cage.

**20:20** · Remember how I was like worried that this was metal exposed? They do have the insulator there, so you would be safe.

**20:28** · And also, I figured out what's going on with this thing. It only helps you with the middle card. It'll pull the middle card out. And it's blue, even though these are hot swappable bays. So, that's kind of odd. Let's pop a card in there and see how that works. Reinstall this thing.

**20:45** · Oh jeez.

**20:50** · Something doesn't feel right. I think it was fine. I got it all attached. Let's see.

**20:59** · We're going to slide into slot two here.

**21:04** · So, I made this mistake last time. You have to like really pull that out otherwise you break this theoretically.

**21:14** · Okay. So, that card is fully in. And now pull that all the way.

**21:22** · I guess that is because there's no way you can get your hands in there and do that in this second slot. But as you saw, I was able to get in here and do it myself. So that mystery is solved. And that actually makes a lot of sense. It is time to examine the artwork. If I was more enterprising, I would really be making posters out of these and selling them because I would buy a poster of this. I haven't gotten into the merch game yet, but I have been cooking up some stickers. So, these are real. I have them. I have hundreds. And if you see me at a VCF event, hopefully you got one. I ran out at VCF Midwest, unfortunately, so not everyone could get one. And I've got another one that I've been uh working on. These are made by yours truly. Feel like I can't sell a sell a sticker pack without three of them, though. You know what I mean? So, let me know if you'd be interested in collab retro stickers. It just feels so ridic I don't know. It's a weird feeling to make stickers about yourself, but let me know. At any rate, one of my favorite parts of servers is all the stickers they put on the lid and especially how they've designed all the information that you need to know like right away when you pull this thing out. You're probably panicking and you want to know like what can I hot swap? My shit's on fire. You know, I I need to solve a problem. Compact was pretty uh sloppy on this one. So, there's literal indentations in the metal for where the stickers go. Little dust there. And this one is like totally off. Obviously, a human has to come along and put this in.

### Lid Diagram

**23:05** · And you're probably pretty tired after your like third one. Every sticker has that little metal indent as to where it should go. Someone tried to steal this one apparently. But they typically give you a pretty good overview what all the LEDs mean. That's what this one is saying. So there's LEDs on the board and it'll tell you, you know, if one of the dim slots is failing. And then we didn't look at it, but this item C is a series of DIP switches that you can configure the board with. So like a power on password.

**23:40** · Hopefully we don't have to worry about that. And there we go. Tape drive installation.

**23:46** · So, you pull out a little plastic piece.

**23:48** · Let's look at that in a second. And bay one can become a tape drive and you can slide it in there. A bunch of the articles I was reading from back in the day, 26 years ago, they thought that was very cool. Obviously, you'd have a lot of tape backups and you could use this particular server to deal with that. And then a good server, in my opinion, will tell you the memory stuff.

**24:13** · So, what the banks are, what's supported. So, it looks like we've got four of these 512 megaby 133 SD RAMs, but it could take two 1 gig SD RAMs. And it's trying to tell you where to put them. So, before we button it up, let's look at this tape drive scenario. This is another blanking plate type thing.

**24:33** · And then this first drive plus this space for the blanking plate can be used for a tape drive. And uh it's not immediately obvious how to take this out without breaking it. I figured out you can, you know, just visually I was looking at it and kind of do one of these and it comes out. And now we've got extra space. So this was right here and you could put a tape drive in there.

**24:57** · Wish I had known that. I would have would have found a tape drive to put in this thing. We'll have to do that in the future. And somehow before I've closed it up, I am remembering to put the Torx T15 service tool back where it was. So, because I'm reasonably confident this thing is just going to start right up. Let's put the case lid back on and then we'll get the drives back in. There's been a little bit of evolution in terms of drive caddies.

**25:34** · I'm not in love with the stuff that Compact came up with. They're way overbuilt, which usually, you know, you would like, but they're like these really heavy sleds. Yeah. Can't say I'm in love with them. The trick is they have this spot where you can push your thumb. So, you slide them in, hold this out, push with your thumb really hard, and you can lock them in. I think it's time to power this thing on. But something exciting arrived today. The rail kit in the compact box, which has seen better days. It was literally dropped off at my house in this condition with this end open, and I just ripped it some more. It's some bubble wrap. Hopefully, it's everything we need in there. I have been fooled before.

**26:28** · Yeah, that's pretty exciting. Again, got to love seeing the compact logo. This is one of those really weird things that is very hard to collect. This is a box for a rail kit for a DL 380 Gen 2, but I can't I can't get rid of this.

**26:45** · So, it'll go on a shelf somewhere even when this thing's racked up. Hopefully.

**26:50** · Hopefully everything I need is in here.

**26:52** · Let's get this thing hooked up. All right, we are ready to monitor the situation down here. This PS2 keyboard and mouse and monitor compact mouse of course are plugged into the server itself. This beautiful compact keyboard and mouse PS2 combo is plugged into the I just barely show you that outrageous dongle on the remote management card. And the remote management card is plugged into this monitor. So, I guess we'll go with this angle.

### First Power On

**27:23** · We'll see how loud this thing is. Let's give it power.

**27:30** · Just the power supplies are on right now. We might get the remote management card.

**27:36** · Can't really remember.

**27:38** · All right, you ready?

**27:42** · Oh, that's nothing. That is so pleasant sounding.

**27:49** · It's possible to get it worse, but Oh, those fans fans are fantastic. I have no complaints. So, these compacts can take a little while. Well, any enterprise server can take a little while to show something over the VGA. Uh, they're getting louder. Of course, they might rev down. Oddly enough, I'm only seeing output over the remote management card. That's kind of weird. Power supply fan failure bay one. That's because that one's not plugged in. We have two processors at 1.2 GHz each. That's wild. You won't be able to, but I can hear the drive spinning.

**28:29** · Not all of them are showing exactly the same indicator LED. The fans kind of have this ro. They're not consistent sound. So, why aren't we getting anything here? What's going on with that? On the monitor over here, we also have a keyboard error. 301 keyboard error.

**28:47** · Huh. So, let's fully unplug that situation. Possible. I've discovered this in a previous video, but maybe that card takes over. Then you don't get any VGA from the unit itself.

**29:07** · We'll see.

**29:11** · I can hear it spinning up each drive one at a time. I think that's what I'm hearing anyway. Kind of a click and then a spin. This keyboard does have lights and it flashed right when I turned the machine on.

**29:27** · But if I plug the remote management card back in, I'm greeted with Iuntu. Oh jeez, what is this? This must be from like 2006 or something.

**29:38** · Okay, I need to plug in a PS2 mouse and keyboard. So, I was trying to power the thing off. Interesting. Starting VMware services.

**29:49** · I plugged in this mouse and keyboard to the remote management card and it seems to be that it takes precedent over the onboard VGA. Let me see.

**30:01** · Still get that keyboard error. Anyway, I clearly didn't wait long enough last time. Let it spin off the hard drives.

**30:09** · See if we get back to Auntu again. So, right now I've got only a mouse keyboard BGA on the remote management, not on the onboard stuff. And whenever I have that situation, I get 301 keyboard error and it doesn't boot to anything. But when I unplug this just VGA now and I do a reboot, get all sorts of exciting stuff. It's initializing the smart array controller, which is the onboard RAID controller for all these drives. It's happy with no keyboard present. It's interesting. It sees one logical drive, so they're raided in some way, all six drives. I can configure the remote insight board if I'm fast enough. Probably have to do that. I want to see what its IP address is. And then it's looking for Pixiboot options.

**31:01** · The system is configured for Windows 2000. That's convenient.

**31:05** · We'll deal with that later. I don't have a keyboard, so I just have to wait.

**31:09** · Well, we've been sitting here for about 10 minutes. I genuinely have no idea how it got into Iuntu last time. I thought this was going to time out and just start booting. I just made a ridiculous discovery. Notice how I can uh navigate this. That's because I have a keyboard working. Let's see what the IP is. 253.

**31:28** · I don't like that. Let's do not explaining myself very well. I am configuring the IP address of the remote management card and my keyboard works.

### Using Remote Management

**31:39** · Wait, wait until you until you see how I did that. So, we are actually going to put this on the retro rack network. 110 should be available. Do one of these.

**31:50** · Okay. Save. Restarting remote insight.

**31:54** · Please wait. I would like to know more about this thing. It's from 2004.

**31:59** · Doesn't look like it. Okay.

**32:02** · Yes.

**32:04** · Grub loading.

**32:10** · Now the machine is calming down. It used to be called Mount Olympus root admin.

**32:24** · Dang, my mouse doesn't work.

**32:28** · Anyway, I don't care about this Auntu install. It's not mine and it's none of my business. I'm going to get Windows 2000 on this. You want to see how you get a keyboard working on a remote insight lights out edition card?

**32:40** · Let me show you. I know everyone wants to see Chloe, but she does not like loud servers.

**32:47** · So, when this thing's on, she does not stay down here.

**32:52** · Is that right?

**32:54** · I think she just farted on me. Chloe, it stinks in here.

**33:01** · So, here's the story. It's clear that when you have one of these remote insight boards in, it takes BGA precedence. We never got anything over the onboard BGA. And I might have already known that.

**33:14** · I know for you guys it's like you watch a video and then two years later you're like, "Hey, that guy already did that."

**33:19** · But I've lived two years of my life and I have no memory of ever messing with this. So, maybe I already knew this. But obviously when this thing is in here, it takes over. It gets BGA. That's probably that special cable that's plugged in if I had to guess. And then you've got this monster D connector. So this thing goes in here obviously. And it's got this T. You can plug a mouse and keyboard in. And what I had done previously, I think I had this one plugged in and I had the other ones plugged in here. And I was able to boot to Iuntu, but I had the keyboard error and nothing. You know, I couldn't type on either keyboard. What you're supposed to do is this thing has all these coming off and you plug it into the computer, just like this. So, when you've got one of these remote insight boards, it takes over entirely. But it does actually make sense. So you have a local mouse and keyboard that's just getting passed through I suspect right to the machine and then this thing as hopefully we'll see later you can type remotely over the web browser and it will also pass that through to here. So I I had kind of like naively assumed because of that ribbon cable. I thought that was pretty hot stuff and because it was taking over VGA that maybe you could have like keyboard inputs from here and keyboard inputs from here. But that's clearly not the case. These are the only mouse and keyboard inputs on the machine and this thing shares them whether it's remotely through the card or through a physical mouse and keyboard you've got plugged in, you know, right here locally. So, I think I understand it now. But the very exciting news is I now have a mouse and keyboard. So, I can set the IP address of this thing. 10010.

**35:08** · I think I set it. So, we will get that on the retro rack.

**35:13** · Let's go see what that UI looks like.

**35:14** · I've done that before, of course.

**35:16** · Not with this particular card. It's newer, I think. And the dream here is that I can facilitate the install from here. I would imagine that was the whole point. And given that this thing clearly never had a CDROM, that must be possible. So, let's find out.

**35:36** · And there is a certain someone down here that is not going to like this. I won't film that. That's cat abuse. Oh, and if that does anything for you, like and subscribe. Hope you like black coffee.

**35:47** · The server is booted back up. It is sitting at the Ubuntu login screen which looks pretty nice next to my Lynxis stack. And I can actually ping 10010.

**36:01** · That is the IP I assigned the remote management card. This is the router of the RetroArack. It's a big Cisco VXR. So now we are going to load up a Windows XP VM.

**36:16** · Start this one up. This is my Proxbox instance. And it is very useful to have old Windows XP VMs for this very purpose because I'm guessing it's probably the only way I'm going to be able to look at that UI for that card. I love Windows XP. Things used to be so much better, but also worse.

**36:42** · It's hard to say. I always like to use the frog, the default frog.

**36:48** · Just thought it was funny. I think I fired up the wrong Windows XPVM. This one's having a little trouble. I have two that I use, XP and XP2.

**36:58** · Uh, XP2 is a little healthier. I also think XP2 is the one that has access to the RetroAct network. So, as you can see, I'm on a 192.1681 network. This is my home network, my uh home lab network. And this particular VM has two network interfaces.

**37:17** · And I physically passed through uh a network interface that can talk to the retro rack, if that makes any sense at all. Yes, this is the right one. Back in the day, I did everything in my power to make Windows XP look like, I don't know, Windows 9X or whatever. The one you saw earlier was the I think it was called the Luna theme or whatever. And you could kind of make it look like Windows 98. This thing is struggling. I have installed so much crap on this thing such as Divoli Net View. That'll be in a video someday. First things first, command. Okay. Can we see that thing? We can. That is the compact on the desk over there responding to me.

**38:04** · We will try our luck with Internet Explorer first.

**38:09** · And it's looking pretty good. I'm realizing I might need to know a user and password or something. It's trying to establish a secure remote insight session over HTTPS. Uh, we will Oh, man.

**38:26** · I'm going to go look up the default username and password. This is This is exciting progress, though. I probably should have done this while I was in the uh configuration area where I was setting the IP address. We might have to do that again. We'll just try admin admin. You never know.

**38:42** · That's not it. I will say it boots up a lot faster than the Gen One. I can say that. Uh, edit user. That's where I bought it from. Administrator. Oh, here we go.

**38:57** · It's beautiful.

**39:00** · Okay, if that worked, it worked. So now what can we do with this thing? I didn't explore this very deeply. Yeah, this is what I was worried about. All I can do is insert a virtual floppy, which is useful.

**39:20** · I'll give them that. But I need to get Windows 2000 on this thing. Let's do a remote console frame.

**39:29** · Is it going to show us auntu?

**39:32** · Oh man, Java. Hopefully I have the right version of Java. Come on. Theoretically, this would show us the Auntu login screen.

**39:44** · It's think it's loading. It's thinking about it. Let's give Firefox a shot.

**39:49** · I feel like having the default user be capital A administrator is a very like Microsoft Windows thing to do. Compact was a big Microsoft shop. I feel like loading Java applet. Yes, this is good.

**40:07** · Always accept.

**40:10** · Yes.

**40:14** · Oh yeah. This is Oh, I mean what what more could you want? Can I get my mouse in it? Can I type?

**40:23** · I can. It's so delayed. This is so good.

**40:28** · I've uh done this before on a Gen One, but it it just blows me away. Also, look at that compact remote insight lights out edition logo. You get the event log of everything the server's been doing.

**40:42** · And the oldest thing it saw was a browser login in 2001.

**40:47** · Probably because it's battery was dead.

**40:50** · What to do now? Should we net boot this thing? Should we slap a CDROM in it?

**40:55** · Either way, I think we got to get it out of this room. To drive the point home about why this remote management board is so cool, I have turned off the server, but it still has power, so I can still hear the power supplies running.

**41:08** · And anyone that's familiar with this isn't going to be surprised, but let's go check out the browser. Because the power supplies have power and they're smart enough to give that special card energy, I can still navigate this website. So, the server power status is off. This isn't too crazy, but I think I can power it on somehow. Virtual power button. Turn server power on. Should get a lot louder in here.

**41:37** · Confirm.

**41:39** · It's booting up again.

**41:42** · So, it's trying to boot. And I'm going to try to power it off.

**41:48** · Uh, it did not attempt to do that. It did it right away. I like that. What's it look like to make a virtual floppy? Talking about MSTOS, Win 195, Win 198.

**42:01** · Yesesh.

**42:02** · Directory server LDAP port. That's relevant for later.

**42:07** · hopefully. Okay, we need to get this thing out of here and we will hook up the remote management stuff with a KVM that I'll show you. Uh, I think I'm going to want direct keyboard access.

### Racking it Up

**42:21** · I'm pretty sure that web console is going to be a nightmare to install Windows 2000 over. So, see what's going on with this rail kit. It's looking relatively promising so far. All right, we even have the cable management guide. A lot more parts than I was expecting. Uh, usually you just have the piece you put on the server and then the piece it slides into that's in the rack, but I've got these extra these are the sliding rails. Clearly got a bunch of ball bearings in there.

**42:55** · Okay, I feel like I have more than I need here. This doesn't feel right. And then of course the one that goes in the rack.

**43:03** · So the the vertical rack would go like this and you' bolt this in. And it's uh you know bent of course.

**43:12** · It also like this. Okay. I'm not I might have to find a manual for this. Just kidding. I'm not going to read a manual.

**43:19** · So this is marked left and right. These are clearly the pieces that go in the rack. So you install these first. It's pretty common for them to label those.

**43:29** · And so this is the right side. So this goes in the rack like this. And I'm fairly certain the sliding rails insert themselves somehow into these little bolts right here. We'll figure that out. And then this thinner piece goes right here.

**43:51** · Oops. By that I mean the other way around. And it's going to slide onto Sorry. Sorry. It's going to slide on like that.

**44:01** · Uh, and I don't have the screws. Maybe I'll steal some screws from the other one if I don't have the right ones. So, yeah, it's a three-piece steel. This one goes in here. Oh, and it actually clicks. It's toolless. Wow. This thing goes onto here, I think. And then you can uh slide the machine in. Yeah. I'm going to get everything put together. I think I'm getting it figured out here.

**44:25** · So, you saw me slot this in. And then the bearing roller. This thing also in a toolless way. It's got a little like retention clip. Slaps onto this. This is the right side of the server. This goes in the rack. And then you would slide the end of this into here. So, you can slide the whole server out and it'll be like that far out. The rack will start here. That doesn't seem right.

**44:55** · Don't seem far enough. We'll see. We'll see. This part goes in the rack and it's got this some sort of mechanism that seems important to let it know that the server is all the way out. It feels like you wouldn't be able to put the server back in with that there.

**45:12** · And then when you slide it out, you get this thing locks the server out. And then you push that and you can slide it back in. So, it's going to be something to to that effect.

**45:24** · Yeah. And this fits exactly in line with this. And it should fit in the rack.

**45:29** · We'll we'll see.

**45:31** · And then I got to release these screws to make this the depth of my rack.

**45:38** · Okay, I'm going to get all this wired up and I'll show you where we end up.

**45:42** · Traditionally, you would just have some through holes, usually two, on the mounting bracket that goes on the rack and you have a cage nut that's in the rack and you'd screw into that through the through hole. These are threaded and luckily it takes the M16. I think this is an M16 bolt that I've got. And you only have one of them. And I guess these little pieces here are uh touching the rack and taking some of the strainer away. Maybe we'll see. This thing's pretty bent.

**46:10** · Both of them are bent. So, we'll figure that out, I guess. Actually, it's even weirder than that. That thing I was trying to screw that bolt into is actually this guy. Got these captive bolts on the front to screw right in to that hole.

**46:29** · So like pressure is holding this in the rack. Unless this is for some sort of special compact rack. These two guys right here are going to sit in the holes that the cage nut would usually go in. This little guy right here. And that's actually what's holding it on the rack. And then you Wow. Haven't seen one like this. I don't I don't think anyway. Making a custom measurement tool to make sure. Put this in the right place in the rack. I've got devastating news. These are too big for my rack. So, these compress like this and you put it in the rack. Rack post over here. Rack post over here. And see if I can show you. You can press that and get them all the way compressed.

**47:17** · There's like a spring in here that's pushing against me. And even that is too deep for the rack I have, which is pretty surprising. And then so I thought, you know, okay, I'll just unscrew those and let it compress the rest of the way. And the one I did that on, a spring went flying out.

**47:39** · So that's not good.

**47:43** · Let me look at this thing a little more.

**47:45** · I'm pretty sure this just doesn't fit in my rack. It's for a bigger rack. Here's the rails that came with the Gen 5, the other compact I was showing you. And it is a different size, so not compatible. It's also sort of weird.

**48:03** · I don't quite understand how it attaches to the to the rack. So, yeah, I don't think that's going to work. But in the box, Matt also had some a universal rail kit. And these aren't great when you're stacking a lot of stuff up, but I think I have enough room. It basically makes like a shelf for the server to sit on. I think that's what I'm looking at here. Also, this doesn't seem right.

**48:29** · What the heck is going on? I'm going to do a little messing around. See what I can cook up here. Chloe and I were looking through the old rail kit graveyard, and I don't have anything that's going to work. And these Gen 5 rails, the rails from the newer server are just incompatible with this server.

**48:45** · It's not going to work. Uh I think these would fit in my rack. Everyone wants a bigger rack, but they're not going to work with this particular server. They won't interface with the part on the front. A bunch of stuff's going to be in the way. And uh I suspect they did that on purpose so that you can't just pull an old server out and put a new one in.

**49:02** · They want you to buy the rail kit, which is So, I think this universal shelf is going to be fine. I don't like that it doesn't extend the whole width of the server. So, only the first five 56s of the server is going to be uh held.

**49:20** · But that's fine. These will fit. I'm going to go install these and then we'll slide her in and get on with our lives.

**49:27** · So, I got that universal rail kit in, and you can see it kind of makes a shelf. And you literally just put the server on the shelf instead of a actual rail system like the other ones are on.

**49:39** · And it's not ideal because this little bit of metal adds up over time. So, you can't rack up a whole rack with just these. You run out of space. Uh, but I think I'm okay in this situation. But, this was clearly made for not this type of rack. It's made for like a compact rack. These look kind of compacty the the finish on them or like a Sun Micros Systems rack. You can see there's a screw holes in each one. Some of the racks have uh built-in threads that you screw into with smaller bolts. And that's what's going on here. So, basically, I had to go find some bigger washers, which might interfere with whatever I put here.

**50:18** · And then I ran out of these bigger washers. So, like one or two of the bolts have washers that are too too small. is pretty whack even by my standards is what I'm saying. But I think it's going to work. And I am happy that Matt had these in here. There's a couple other items in the rack that I'm using these on, but I uh didn't have anymore. So, we are making progress.

**50:36** · Let's see if it fits. I'm a little worried about width. I tried to push these as far out as I could. Let me get the server.

**50:44** · Is there hope?

**50:47** · Come on. Oh, come on.

**50:51** · Yeah, I think it's going to work.

**50:56** · Probably doesn't go all the way in. No, it does. All right. I got a smile on my face. That look that looks pretty good.

**51:03** · And it fits. And uh that other slot there. We might have to move things up and down a little bit to get whatever I need in there. I'll probably redo this whole thing down here anyway. So, yeah, that's looking pretty good. I have to step like way back to even show you the whole thing. And you can't even see everything. That's looking pretty good.

**51:22** · Let's talk about the KVM situation. So, this is my KVM setup, meaning I can plug a monitor, mouse, and keyboard into it.

### KVM Setup

**51:29** · PS2 mouse, and keyboard. On the other side of this wall is the workbench where we were looking at the server. And you can see exactly one wire, one cat 6, well, cat 5 or whatever it is, cable is going over this trunk. And at the end of the day, it goes into the server room that's on the other side of that wall.

**51:49** · On the other end of the Cat 5, Cat 6 cable, you plug in one of these things called a server interface pod, and it has the VGA and PS2. I have a only a couple of these. I've got a ton of these VGA to USB server interface pods, but I don't think that's going to work for us. So, we're going to plug this into the back of the compact server. And you'll notice I only had one cable going over to the server room. So, at the moment, I can only plug in one of these. So, I wanted to explore this. I did this in other videos, but I never got it working quite right. Uh, and you'll notice it's not in here the device I'm looking for.

**52:32** · It basically looks like a switch or a hub. So, you plug the KVM into this thing and you can plug a bunch of these in and theoretically control a bunch of servers, but I can't find the damn thing. So, I was rifling through a box on the other side of the room, like digging my hand in there. And this was in there. Some 3D printing stuff, some accessories that came with a 3D printer.

**52:58** · And I'm pretty sure I jammed my hand into this thing. And it's like a two millimeter cut. Didn't even really hurt.

**53:07** · And I could not get it to to stop bleeding. So, this is going to be really alarming for someone that's never done this. I literally had to super glue it shut because it would not stop. I mean, you could see the blood there. It would not stop bleeding. This is like the fourth or fifth band-aid I've had to put on here. And so, I finally had to superglue it and it stopped. So, that's going to be fun to pull off tomorrow.

**53:28** · So, yeah, everything's going really well. Let's plug in this VGA and uh PS2 server interface pod. And we should be able to see the VGA output and input into that remote management card from in here on a mouse and keyboard and monitor. It's the next evening. Going to see if this KVM is going to work for us.

**53:48** · This monitor and the mouse and keyboard are plugged directly into the KVM that's just on the other side of the wall. And right now it does not see any pods. The way this works is this means uh you're not latched on to any of the pods that might be plugged in and you can hit control I think twice or three times. Twice and then you can choose which server hopefully when they show up uh you want to get in on. Pretty sweet. Oh, and I found the thing I was looking for. So, you can plug one cable coming from the KVM and you get access to eight machines off that one Ethernet cable, which is for Cat 6 cable or whatever, which is pretty sweet. Uh, but this video is kind of getting out of hand. And we we have a long way to go.

**54:33** · So, we'll save this for another video.

**54:35** · And look how small that cut was, but it was super deep. Super glue saves the day. I cannot tell you how much that was bleeding. That was wild. Okay, the server is hooked up in there. The remote management card has network. The onboard nick has network. The VGA and PS2 stuff is coming from the pod into our special little dongle thing that the card uses.

**54:58** · I'm able to remotely power it on with this PDU I've got in the rack. I've got this little Go app I use to control that PDU. And you can see I've labeled outlet 8 as DL380 Gen 2 cuz that's what it's plugged into.

**55:12** · So, we will flip it on. And I'm hoping we see a pod show up over here. I don't know if the server is going to boot up all the way. I've been not shutting it down gracefully. And I think when you do that, it tries to rip back on because it thinks some hard crash took place or a power outage or something.

**55:30** · So why don't you see anything? Let me let me go make sure that server is actually on. Chloe, go figure it out.

**55:39** · So I went in there. It definitely has power as in the power supplies are on.

**55:44** · So, probably not too surprising that I'm not getting in here yet. But the remote insight board is working. I can navigate to the web UI again. So, I think what we're going to do is power it on all the way here.

**55:58** · And right away, we can see our little pod. I usually use this one on a server that is an HMC, some IBM stuff. And if we get in here, can we see? Yes, we can see it booting. And we can access it from here.

**56:13** · sometimes this monitor.

**56:16** · Yeah, sorry about that aspect.

**56:20** · Chloe. So, yeah, we can operate the machine from in here. And of course, we have the remote management card to help us out a little bit. I was looking into what it would be like to pixie boot that thing, aka net boot, and install Windows 2000 over my network, and that seemed pretty involved. Uh, I think that could be a whole video. So, what I'm going to do is I'm going to scrge up a CD or DVD ROM, whatever that thing takes, and then uh we will pop a Windows 2000 server CD in there. We'll go from there. I guess I didn't actually try pressing a key. F1 to continue. Oh, yes. And then you can get out of this thing. And assuming I had multiple machines plugged in, you can transfer between them. It's actually pretty sweet. When I was plugged in directly, it also did this when I was trying to get into Auntu.

### Attempting Windows 2000 From CD

**57:09** · It might come back. We'll see.

**57:13** · Actually, that's weird that the KVM gave up.

**57:16** · Nothing's ever easy. And we're all good.

**57:19** · It came back.

**57:21** · And there's beautiful Abuntu. Side note, hopefully not foreshadowing. I have never seen a mouse work on this screen.

**57:28** · The KVM, the remote management. I can't really remember if when I was directly connected it was working.

**57:35** · That would obviously be a massive problem for Windows 2000, but we'll see.

**57:38** · We'll see what happens. Okay, I'm going to kick off a Windows 2000 install hopefully. Chloe helped me steal the CDROM drive out of that Gen 5 I was showing you earlier. It does appear to be a compatible part. And as you may have surmised, in order to accomplish this, I had to take the server out of the rack again because I'm a complete idiot and should have done this earlier.

**57:56** · Let's burn a copy of Windows 2000 advanced server to a disc. I've heard that's the best one.

**58:01** · But the cream will rise to the top. Oh yeah.

**58:08** · While that's cooking away, I'm trying to see if that CDROM gets recognized. Don't know if I got in here fast enough. Nothing indicating that yet. So, you'll notice there's no like F2 to get into a BIOS. It's an absolute nightmare to actually use these things. You have to use something called a compact smart start. Might have to burn another CD. We'll see. This is that onboard compact smart array 5i.

**58:42** · And it is the RAID controller. And you can see we've got a RAID 5 340 gigs. And it's pretty nice that you can actually get in here and mess with the logical drive. I'm hoping we can leave that if we can get the thing to boot from a disc uh of some sort. And then I don't think we get a lot of help in the lights out thing. Yeah, it's just everything we already saw.

**59:09** · And there it's trying to pixie boot over that network card. Let's see if we can take a look at the ROM based setup utility. Oh, there's a chance it will try to boot from the CDROM first. I've never seen anyone except for IBM reference IPL. That stands for initial program load, or at least that's what IBM calls it. And that's from way back in like the mainframe days. That's just what they call booting an operating system essentially. So, this is good.

**59:36** · Sorry, I'm just messing around here. I hadn't been in here yet. Well, that's promising. It might just boot from the CDROM.

**59:43** · That's good.

**59:46** · I just barely missed it. It's getting into Iuntu here. Right before it hits the grub loader, it does say attempting to boot from CDROM and then and so on. Feeling cautiously optimistic. The CD is almost burnt. See you later, Iuntu. We've got some excruciating Windowsing to do. Just put the CD in. Usually you can restart from here.

**1:00:08** · Oh man, I just want to I can't tell when it's actually getting to options. I think I can restart with that. I'd really rather not reboot the machine. There we go. Restart.

**1:00:22** · Will it work? The hell is this freaking Linux?

**1:00:29** · Yeah, I want to restart the computer. I don't want to like restart Gnome or whatever. Get out of here. What?

**1:00:37** · Any What is this? Obviously, it's a skin on top of the existing Iuntu install, but I'm not going to look it up.

**1:00:45** · Hello.

**1:00:47** · work.

**1:00:48** · I can't help you. The machine is powered off. And I don't know if it's obvious, but the computer right behind me is where I'm using the Windows XP machine to turn it on and off remotely. And uh yeah, this combo of KVM plus the compact remote insight lights out edition card is pretty primo and I think it's what a lot of people would have done so they could remotely manage these things. So, let's turn this thing on and then over here right away we can see it.

**1:01:19** · Oh, I haven't showed you that yet. That's the best. We caught this early. That's nice. I'm going to let this thing do its whole thing and get all the way to almost getting to Iuntu and it looked like it was going to attempt to boot off the CD. And I don't know if I told you, but I put the Windows 2000 CD in the drive. And I believe it's a bootable CD.

**1:01:39** · So, let's see what happens. I think you get a little spinner for each drive. So, that's all of our six drives. Don't think we're going to get another one. I like that. I think that's really cool. All right, we're going to F1 to continue with regular boot. And right here, yes. Press any key to boot from CD.

**1:02:01** · Yes. Okay, let's go. This video is already an hour long. Do you think I can like push it to two hours? I really like long form content. So, I'm really proud when I get a video that's like an hour long.

**1:02:17** · Everyone's like, "Hey, hey, you should post shorts. You should post 20-minute videos and stuff and maybe your channel will grow more." And like, maybe it will, but then I won't have fun and I'll stop doing it. This is going to be a long one because we're going to get Windows 2000 installed. And I'm sure this is going to fail like in 30 different thousand ways, a million times, and I have to start over a million times. Uh, I'll bring you along for the ride if that happens. And then once we do that, we got to learn Active Directory, which I don't really know.

**1:02:43** · So, should be fun. At any rate, I suspect Windows 2000 has all the drivers we need for this bad boy. And I'm going to let it do its thing. I'm going to get through a Windows 2000 install. And hopefully, unless anything interesting happens, which means something terrible happens and this video becomes 3 hours, I'll bring you along for any disasters.

**1:03:04** · But I think it's going to be okay. I'll get it installed and I'll bring you back. So, that big spiel I just gave you about how easy this is going to be. Setup did not find any hard disk drives installed on your computer. It's okay.

**1:03:15** · It's not your fault. Dell monitor that always works. Dude, screw compact. I have calmed down. So, I'm in the RAID controller configuration and I'm going to make a new smaller RAID out of just the Seagate drives which were in the bottom because maybe someday I'll use that top left port for a tape drive. And so we're going to make the theory here is I'll make a smaller array that's brand new with nothing on it and maybe that will confuse Windows less I guess we'll see. So here is all three Seagates in a RAID five I guess which is perfect. I think Windows can handle that. It's probably a driver issue though. I I'm actually really surprised that that Windows 2000 with Service Pack 4 didn't have these compact Smart Array 5i drivers, but that's what we will attempt to do next. So, now we have a 136.7 GB logical drive. Let's get out of here and try the Windows installer again. I think I just missed it, but right when you start up, you can press F6 to install a third-p partyy RAID driver or SCSI. And we might just need to do that. And that's as simple as getting the drivers on a floppy disc, but we have a floppy drive. So, I think we'll be okay, but we'll see. That didn't work. That's okay. We will revisit my Nemesis Smart Start.

### SmartStart

**1:04:53** · This one's too old. This came with my remote Insight board and it's for Windows NT or that's the era it's from. It's got a copyright 1997. So, I need to go find what would be era appropriate for this Gen 2 and Windows 2000. I might have a physical one floating around here somewhere that's useful. Let me go see. I'm burning a copy of Smart Start 5.5, which was released August 27th of 2002.

**1:05:20** · And if you want to feel real bad here in 2026, that was 24 years ago, unfortunately. I was there. I was around. I think I was 14.

**1:05:29** · Anyway, I'm burning this twice because the first one didn't work. And I downloaded this off HPE's website and uh it supports Windows 2000 Advanced Server. Feel like that's a really bad name. They should have called it Windows 2000 Server Advanced. What are they doing? I'm almost positive I have a copy of 5.5 or 6 something, you know, in the case and everything, and I cannot find it. Didn't cut myself this time. I'm pretty sure it's in my storage unit. And uh that's that's a whole thing if you want to go get on the Patreon and see what that's about. Should probably go uh give that thing a visit. I'm waiting for that CD to burn and I have nothing to film.

**1:06:11** · Chloe, what are you doing, dude? Chlo, Chloe, she loves these boxes. Those are uh What are those? G5 power PCs just pre- floating with Chloe over there. What's the plan, dude? What's your plan?

**1:06:30** · There is no plan. All right, we're having fun. The smart start CD I just showed you, it's in the drive. We're going to get out of here. We can fast track that. Are we just going to get three of these now? Two. Yeah, flash the third one and then was super quick.

**1:06:50** · Okay, let's keep going. Oh, yes. Get ready. I really hope this mouse works cuz I'm pretty sure this is an like a uh slim down version of Windows 95. Please work. Yeah, this is a big I have like vague memories of dealing with this before. This is going to be really rough without a mouse. I wonder if the onboard card has a mouse problem. Maybe hopefully they did it in an accessible way.

**1:07:24** · Then we'll deal with that later. We will let it think it is June 15th, 2001. That is good enough for me. Windows was really good about this this accessibility layer where you can do everything with the keyboard. I wouldn't say they were the pioneers of it, but they kind of made it the standard. And you got to respect them for that. You're like, I'm going to do this whole thing without a mouse. It's going to be a nightmare and we're going to have to figure that out. But I think it's going to be possible. Okay, here we go.

**1:07:54** · Because I don't have a mouse, I am just going to What is this? Please insert a server profile discet. I'll just put a blank disc in, I guess. Pull one off the old stack. I need to buy another one of these. This guy is still selling these.

**1:08:08** · I bought this like four years ago. Look how beautiful this server profile discet thing is. My favorite thing about Compact Smart Start is forgetting everything I know about it that I've learned over the last 5 years and then just assuming it's going to work with whatever I do and it never works out. So, I put a blank Oh, that's right. My mouse doesn't work.

**1:08:30** · What I was saying is I put a blank disc in the floppy drive and I pres you know, it'd be nice if you could move the mouse, but presumably it's like formatting it or it's doing something or it'll just freeze, you know. And then we'll start over. I should have skipped this whole thing. It doesn't like it.

**1:08:50** · Can I skip this somehow?

**1:08:53** · Oh, I should really figure out why this mouse doesn't work. All right, so going the manual configuration route hangs the machine locks up. And again, no mouse, which yeah, we might have to pull out the remote management card. That might be the issue. I don't think it's the KBM.

**1:09:16** · Not our not our little Dell here. I don't know why it just lost signal when I did that. Why do I mess with these freaking compacts? Use the grease weasel here to write out what is hopefully a server profile discet. In the zip file that contained the smart start ISO that I downloaded from HPE, it had a SPD.

**1:09:34** · file in it. And you are supposed to place that on a newly fresh formatted floppy disc, which is basically what I've just done. Kind of interesting. The spd. any file is just an empty file. So either that's bad and it's not correct in the archive or it would have been nice if uh the software was just smart enough to place an empty SPD.in file on a formatted floppy. But get this in the machine, see if we get any further. Got the machine booting back up here. And I'm also running an experiment. I'm plugging the KBM mouse directly into the machine and it's bypassing the remote management card. I'm hoping that's what's causing my mouse issues. Windows 2000 does support USB mice and keyboard.

**1:10:12** · Obviously, that doesn't help us until we get Windows 2000 installed. So, we should be able to do everything with just a keyboard if we have to. Smart start loading up here. Will we get a mouse? We do. That is one problem solved so far. That is nice to see. I am going to do guided. Um, historically, I've been told not to do this. Uh, that was with the older gear in Windows NT4.0.

**1:10:38** · I'm going to give it a shot for this Gen two and Windows 2000. And I'm going to go push the floppy disc in. We'll be right back. It's in the floppy drive now.

**1:10:49** · This is what I was talking about earlier where it kind of hangs. And if I had a mouse, I could tell if it was totally hung. It's not. It's taking a look at that floppy disc apparently. I mean, are we even surprised here? I verified what I wrote to that disc too with the grease weasel. Let's try the manual path one more time just for fun. Wasn't working last night.

**1:11:07** · Seems like this is really what we want because it's the path that lets you use the vendor CD. So that's that Windows 2000 CD I already burnt. And I think what it does is it lays down a little maybe partition or something with all the drivers you need for this machine and then the Windows CD is able to use it. Now, if this doesn't work at all, I can go find the drivers for this RAID controller, get those on a floppy disc, and then we'll be able to manually insert them as part of the Windows 2000 install process. I don't know what this thing's doing. Well, it advanced. Uh, this would just restart the whole thing or hang up the machine last time I tried it the other night. Microsoft.

**1:11:46** · I was really I was getting worried that advanced server wasn't supported, but it is. I just got to find the English version. There we go. This is going to automatically configure the system. It did say it was going to reboot and require input from me. So, we'll see.

**1:12:02** · Nice. It was just looking at the array controllers. Here's the array configuration. I just deleted and then recreated a rate array. So, it's saying something's happening in the background and we're going to have slightly lower performance. Let's see what it sees. It wants to use all the drives.

**1:12:21** · Let's create a new array. So, yeah, now we've got two of them. That's fine. I know I talk a lot of crap, but if you knew what you were doing, this was probably pretty slick back in the day. So, we've got two logical drives now. That's fine. We'll install Windows on one of them. Uh, I think you just close this.

**1:12:44** · Yeah, fully rebooting this time and initializing all the drives. Two logical drives. That's good. F1 to continue. And it should boot from the Smart Start CD again. I think it did. So presumably we're going to see some next steps. I wonder where it stores its progress. It's kind of interesting to think about. It showed the splash screen and then it did a full reboot again. Maybe that's fine. I don't know. Nice. It's doing something.

**1:13:13** · Yeah. So it lays down this little system partition and writes a bunch of drivers to it somehow. And I think at a certain point it will ask me for the Windows 2000 install media. Compact used to charge you for copies of this smart start because obviously it's like a version of Windows 95 or something. So they had to pass that cost on to you. Okay. Create diset images of support software. I think we're going to want to do this. It says I can skip it, but I I think I'm just going to do it even though it's going to take forever.

**1:13:43** · One day it'd be wild. You can set up a integration server running some compact software. And so you're saying I'm going to get mine from the CD that's in there.

**1:13:53** · You could have a a server you were talking to. That'd be a wild adventure, right? So, it's like which which drivers do I care about? Probably the support pack. That seems important. Drivers that can be used to complete the OS installation. Feel like I'm going to make a mistake here.

**1:14:14** · Uh, you can upgrade the firmware. I don't care about that. I think we're going to go with that. I'm going to put a fresh disc in and let it do its thing.

**1:14:23** · Here's a spicy revelation. That floppy drive might be bad. It sat here for a few minutes and gave up and couldn't build it. So, that might be a problem to solve. So, now I'm going to take the Smart Start CD out. I'm going to put Windows 2000 in after I do that. Windows 2000 is in there. Not feeling good about that floppy drive maybe not working.

**1:14:49** · It's It's simple enough to find the drivers and write them to a floppy. Compact actually had little utilities that'll still run on uh Windows today. We'll see. Maybe we'll need them, maybe we won't. I don't know. We are getting in here. But as you know, you don't find out if it can't see drives until much later. The moment of truth.

**1:15:09** · No hard drives. Hi, Chloe. So, it is possible that it did actually write the files to that disc and it just kind of looked like it failed. I kind of wasn't really watching. So, we're going to get back into Windows 2000 and I'm going to hit F6 to see if any useful drivers are on that disc and we'll we'll go from there. We need the driver for this compact Smart Array 5i controller.

### Trying Storage Drivers from Floppy

**1:15:36** · She's not impressed.

**1:15:38** · I hit F10 before letting it boot and you can get into the system partition utilities. I think this is what the smart start laid down somewhere on some disc. Maybe you're supposed to do it this way. No, I don't like this. I don't think it's going to help us very much at the moment. Let's get back into Windows 2000.

**1:16:01** · So, here's where theoretically we can use that support disc. I pressed F6 as we came in and we choose special. Please insert disc labeled. Let's see if it reads it.

**1:16:14** · I don't know. Failure. I can try writing these to a disc on the grease weasel. I will do that now. The drivers are on this disc. I read them back with the grease weasel. I can see them. I made an image of the drivers that I downloaded for the array controller. Then I realized we should be able to insert a virtual floppy with a remote management card. So I found the Smart Array 5X drivers for Windows 2000. It's this little executable, but you can extract all the files. And so I made a floppy disc image with everything Windows 2000 should need. And then back over here, we should be able to insert it. I made an IMA file and an image file. They're identical. Uh I don't know what this thing's going to like.

**1:17:01** · We'll see. Bad floppy image. Come on.

**1:17:05** · That was the IMA. Let's try the img file. Okay. Uh, let me go figure out what type of image files it wants for this thing. I had used win image to create these images. I'm going to try this HP floppy image application that having a hard time believing it's going to run on my machine here. No floppy drives detected yet because I don't have any. Probably wants to read from a floppy and produce the image, unfortunately. We could pull out another machine for that. I'd rather not. Let's go see if the drive works. I'm going to pop that disc in. Disc is in. This has just been sitting here waiting for me.

**1:17:44** · Can it read it? No, it cannot. The adventure continues. This thing has a real floppy drive. I'll get the drivers on here and it should be able to write to the floppy and then so we'll basically we'll make a a physical one just in case all this funny business I'm doing with the grease weasel uh is confusing things and the drive in there is actually okay and then we'll run the utility that the compact disc get utility or whatever which should theoretically be able to make us some image files that are compatible with the remote lights out thing. Okay, just getting everything ready here. These are the drivers. I don't really know what happens if you click install. I've only extracted. And then this is the compact remote insight disc image utility. And I can create an image file from a drive.

**1:18:27** · But you'll see there are no drives. And there's literally a floppy drive in there. But Windows doesn't see it. So, let's figure out what's uh going on with that. Well, there's your problem. There's no power to the floppy drive. This is giving me a vague memory.

**1:18:50** · Did I do that on purpose for some reason? Is this floppy drive a troublemaker?

**1:18:56** · Let's find out. Want to see the weirdest thing about this IBM computer? This is a pennium for IBM computer. And you plug it, you give it power and then it suspiciously revs all the fans and then turns off. It always does. I don't know why it needs to do that. And then you hit the power on the front and it actually powers it on. Super weird.

**1:19:21** · It's very uh jarring the first time you experience that. Still doesn't see it. My money is on. I probably knew that floppy drive was broken or something and just decided to disconnect it from power. Don't worry, though. We'll just uh do this.

**1:19:39** · That didn't work. And this machine's behaving really weird. It won't show me a post screen and I can't get into the BIOS to see if the floppy drive's disabled or something. Mess around a little bit. This monitor was too slow to sync and see the post screen, but I could get in with this one. Right over in devices, the discat drive is disabled. I would like it enabled.

**1:20:02** · Wonder if I did that. Wonder what's going on here. This thing has a really cool post screen. Oh, I heard the drive clang. And Windows is booting. So, I bet the one in there works. Let's find out.

**1:20:16** · We're back on track. The floppy disc in there works just fine. In fact, it can read the floppy I wrote with the grease weasel that the server is having trouble with. So, let's see here. These are the drivers. I don't know what install does. Okay, that would actually just install them on this system.

**1:20:33** · So, huh. I guess our best bet is to see if we can just make a virtual image with this thing. SSA drivers and maybe this will magically do it in such a way that the virtual floppy likes. And that's still assuming that everything on there is even what I need.

**1:20:53** · We'll see. This is the one we just wrote. It does report one kilobyte larger than this one. They are slightly different. I did check that this was the exact, you know, amount of bytes that a 1.4 megabyte floppy should have, and it was. Um, let's try it out. All right, Chloe. Chloe is helping.

**1:21:17** · Insert virtual floppy.

**1:21:19** · Let's go. Come on.

**1:21:23** · It worked.

**1:21:25** · One second. You can give it a boot option.

**1:21:31** · I think I'll just submit. I guess it's it's in there. It's in there. There is a tiny glimmer of hope here. This is all assuming I wrote the right files in the first place. So, let's see if we can get back into the Windows 2000 installer. I will hit F6 and we'll see if it can view the virtual disc. I wonder if it's one of those things like the VGA taking over. The physical floppy doesn't work when you're using the remote insight board. Maybe I should read the manual. I was too busy talking, so I missed the part where I needed to press F6.

**1:22:05** · If you're ever doing this, you It asks you to press F6. It gives no indication that you've done so. Then it continues to load the rest of its files. And if it registered your key input, you will get that special screen that lets you read from a floppy disc. Okay, I can say S. It should theoretically be virtually inserted.

**1:22:24** · No, come on.

**1:22:28** · What is going on? What a disappointment. New theory. The virtual floppy is drive B. Maybe I can disable the onboard floppy somehow. Wouldn't really want to go unplug it because I would have to take the server out again. But we will do what we can.

**1:22:46** · Man, these compact machines really make me feel like an absolute I I can't tell you how many hundreds of times I've installed Windows on hundreds of computers since I was a little kid. And it is so difficult on these things. We're also so close. We just need these drivers. The system definitely knows that that physical drive is there.

**1:23:08** · Let's try something and see if it'll just boot off of a floppy disc. I think it's first in the boot order. I will say I am having fun troubleshooting this, but this video is like an hour and a half long already. I'm worried about uh your guys's sanity. You got to go through a lot of reboots to troubleshoot. I think anecdotally, it does boot faster than the Gen 1's. So maybe that's why I'm having a little better time here in terms of my own happiness. So when I tell it to continue, it'll flash that it's trying to boot from A really quick up here and then it continues to the CD. We're hoping it can do that.

**1:23:47** · I don't know what it's doing. The KVM's locked on. No, it it skipped that that disc. So, we have a physical drive issue. I'm going to pull the machine out. I'm going to unplug the floppy drive and uh see if that promotes our virtual drive to drive A. Coming back up. The floppy drive has been totally eliminated from the situation. I unplugged the cable.

**1:24:26** · Hopefully, it's okay booting that way. I would hope that the virtual one gets inserted. I'm not really sure at this point. Inserted that image again virtually and it doesn't even try to read it. So, previously it would go off and try to access the physical drive.

**1:24:42** · Oh man, this drive active thing might be a clue. So if I eject this, now I don't have an image inserted and the drive active says yes. What's up with that? If I put it back in, now the image is inserted, but the drive is no longer active.

**1:25:05** · What does that mean? I just went on an adventure the last two hours. But as you can see, we have the enduser license agreement. I'm going to agree with F8. We're getting further than we've ever gotten before, and it can see the discs. We're about an hour and a half in. Let's let Macho Man Randy Savage describe how I felt the last two days.

### Slipstreaming Drivers

**1:25:31** · I'm wondering if you ever cry. You ever has Macho Man ever cried?

**1:25:34** · Oh, yeah.

**1:25:35** · Really?

**1:25:36** · Uh-huh. It's okay for macho old men to show every emotion available right there. You know, because I've cried a thousand times. I'm going to cry some more.

**1:25:43** · We are definitely not out of the woods yet. But we have finally gotten the Windows 2000 installer to recognize that Smart Array. I used a tool called Nite, and I actually slipstreamed the drivers into the installation media, the ISO, that I burnt to a new CDROM. And this is the outcome. There's There's hope. Let's use this unpartitioned space. So this ISA utilities thing, that's what Smart Start laid down. Uh we're going to use this one. I guess we are going to go NTFS. That is totally fine. Wow, there is hope.

**1:26:22** · There is hope. Formatting is cruising along. This is usually pretty quick and it's getting kind of late tonight, so see how long this takes. Setup is copying files. This is one of the most beautiful things I've ever seen in my entire life.

**1:26:37** · There's a chance.

**1:26:39** · There's a real chance we're we're gonna well at least get Windows 2000 installed at this point. Like I was saying, might as well make the three-hour video. Oh yes. My name is Clab and I am part of the Collab Retro Organization. Oh yes. Let's go find one of these. Dang it. There's actually another part of that macho man clip. It's very important.

**1:27:05** · And I'm going to tell you something right now. There's one guarantee in life and that there are no guarantees. Yeah. And understand this. Yeah. Nobody likes a quitter. Nobody said life was easy. So if you get knocked down, take the standing eight count, get back up, and fight again.

**1:27:24** · We're in. We will accept the five concurrent connections. Default. Oh boy. This is the DL 380 G2. Did anyone else like that that Microsoft used like the X's for the password inputs and they were kind of like too small and up towards the top? Just realized and I always kind of like that.

**1:27:46** · Oh boy. What do we need? We're going to need IAS. We're going to need networking services. Terminal services. That seems pretty good. I don't think I care about terminal services licensing. I would like to stream multimedia content to my network users.

**1:28:02** · Should honestly just probably install all of this, but you can install it later. It is Saturday, June 16th, 2001 at 2 a.m. Luckily, it's not actually 2 a.m. Not yet. Anyway, oh, we're not in Pacific, though. Mountain time, baby.

**1:28:18** · It is installing all the crap I told it to install. I think it's booting. The CDROM is out. I don't think it's booting. I know it's trying to boot. Oh yes, I'm like 5 days into this project, man. These damn compact servers and it was just a driver issue. So I'd never slipstream my own drivers before. And that was really, really, really easy.

**1:28:46** · Took me, I don't know, 10 seconds of Googling or whatever. Also, this like terrible display on my VGA is because my VGA cables are so thin. It's really hard to find good VGA cables that are, you know, made in the '9s or whatever. So, if you got a lead on those, shoot me an email.

**1:29:05** · Oh, it's like particularly bad. But, let's see here. The first log on. Does the mouse still work? Oh, yeah. This monitor's having trouble. Can't see the start bar. The combo of the KVM. Let's see if we can auto adjust. Maybe. Yeah, we found it. Still a lot of flicker. It's terrible. This is asking us to configure. Oh, you can just go right to Active Directory. Oh, this is a install with SP4 Service 4, by the way.

**1:29:35** · So, it's going to be a little better than the OG 2000 experience.

**1:29:40** · Okay, it's not 2 a.m., but it's not ideal for a Tuesday night for me. I got a real job. I'm going to leave it here and then we're going to install Active Directory and we'll try to get some other machines using this well that DL380 Gen 2 as their uh dom I guess I guess you could call it a domain controller. It's the Anyway, we'll find out. I cannot believe that we are here and I can use this machine. What kind of games we got on this? Nothing.

**1:30:15** · Never mind. There it is. Pinball. Let's load a pinball. Oh yeah. Oh yeah. Haven't played this in a while. I don't actually remember how to how to play.

### Installing Active Directory

**1:30:34** · How do I move the things? Anyway, I'll be back tomorrow. Today is finally the day we are going to become active directory experts. And the moment I get frustrated, we'll be cracking into these. I suspect that won't take very long. I've drank a lot of beer in my life. I don't know if I've ever seen a Tall Boy six-pack that comes in a cardboard case. I guess that's fine. The first problem to solve is this particular monitor really flickers and basically looks like crap even in person. If we hook it up to this one, it doesn't flicker. But this one's getting tired. It's got a bunch of screen tearing going on. It's coming from the same source. It's these thin cables.

**1:31:19** · That's my That's my guess. You kind of move them around and the screen changes out of the KVM. I'm going into this uh what is this thing called? VGA duplicator.

**1:31:28** · So, it's a powered splitter. And it really makes them look a lot better versus a passive splitter. But yeah, they still kind of look like crap. So, let's go into a HDMI VGA to HDMI capture setup. See if that's any better. It's not going great. Both cables with HDMI out make this terrible flicker. And I can't even get OBS to lock onto this thing.

**1:31:55** · I mean, it's only ever going to be so good coming out of the KVM, but this is not ideal. And this monitor, even when I adjust the theoretical aspect ratio, uh, we're we're totally off. So, this one's this isn't going to work. This one doesn't flicker. Does have a little screen tearing, but I think we can live with that. But still that same offset start button. Yeah, this is going to be our best bet. Let me go find the stand for this. These videos would be a lot shorter if I didn't bring you along for the ride, but we don't want you to miss out on anything. Check out this new battle station setup. Let's uh change the resolution on this thing, shall we?

**1:32:35** · Oh, it's already pretty high.

**1:32:38** · That's not going to help, will it?

**1:32:44** · It kind of does. Yeah. 800 by 600. That's what we're going with.

**1:32:51** · Wow.

**1:32:53** · Yeah. Let's find the uh let's find the stand for this thing. Chloe likes it. It's got a cool new fort. This thing is unmounted from its stand because it used to be on like a rotating arm on the desk.

**1:33:05** · Always put the screws. Just tape the screws to the thing and then you won't lose them. It's back in its stand via what I can only describe as an insufficient amount of screws. It's not perfect. That side's cut off a little bit, but we can see the start button and we just have like minimal screen tearing. So, yeah, I think this is going to be sufficient. I am already frustrated though. Not Microsoft's fault.

**1:33:31** · Step one is does the network card work?

**1:33:36** · And it does, which is a small miracle. I thought maybe I would have to deal with some more drivers. Now I get this splash screen and I can do Active Directory and I can make this server a domain controller. Uh that that feels like cheating. I want the OG experience. So, command prompt. We're going to do DC promo.

**1:34:01** · Yes, the Active Directory installation wizard. Let's see. I'm going to make a new domain. Uh, I should explain what I'm doing here. Domain controller came from the Windows NTS, I believe. And then Active Directory replaced it. And so, you know, when you're like in school and you had to log in with your user and password or at work on a Windows machine. Basically, this will be the server that controls all that. I'll be able to set up users and allow them to log into the network. I'll be able to lock down like a Windows XP machine hopefully. And you'll have to log in when this thing's on. And that's that's the dream here. Uh, I'm creating new I'm going to create a new forest of domain trees. Building a forest DNS for the new domain. We're going to do clab retro.local.

**1:34:47** · See how that treats us. It's thinking about something. So, this thing needs to be a DNS server. I believe I can use my existing DHCP server, which is that Cisco VXR running in the retro rack that this thing just pinged. And we'll see. Right now though, it's just thinking.

**1:35:04** · So, we'll be back. The net buoy name, net bios name, collab retro is fine. be really crazy to get this thing serving some old Windows PCs over token ring.

**1:35:16** · That could be fun. We're going to leave the defaults. I'm going into this relatively blind. I watched like one YouTube video. I'll show it on the screen here. Install and configure a DNS server on this computer. I thought it was going to ask me to do that. Oh, okay. Cool. It is asking me. Yes, install and configure DNS. This is probably going to be the hardest part.

**1:35:38** · What we got here? So, it's asking me for backwards compatibility. And like I was just alluding to, I do have some Windows 95 and Windows 98 machines. I think it's going to be easy if we just go easier.

**1:35:50** · If we go Windows 2000 and up, that's what we're going to do this time. Maybe we'll redo this someday. Give it a password, I guess. Configuring Active Directory. This process can take several minutes or considerably longer. I really like this animation back when computers did real work. Says it's done. And a note for the eagle-eyed, that's not actually the time right now. It's considerably earlier.

**1:36:13** · And also, I edit as I go. So, there might be massive gaps between what you're seeing and what's going on down there. Now, the real fun begins. I think it has been sitting here at preparing network connections for about over five minutes. It got over that. So, now if I go to Active Directory, I can manage user accounts. And I am going to cheat. I'm gonna click this. Didn't do anything.

**1:36:44** · Yes. So this is my domain and we need to add some users. New T. I don't know how to do this. New user. Oh yeah, dude. I will add myself. My loon is going to be collab retro.

### Setting up DNS

**1:37:03** · Excellent.

**1:37:05** · We'll set a password. I will say the password never expires. So, there's a lot of You could do a million videos on Active Directory. There is a lot of flexibility.

**1:37:14** · There's me.

**1:37:18** · Let's add a new user. My most important employee. Yes, it's Chloe with a K. And if you don't have a sense of humor, you're not going to like her last name. Let's call her Chloe K. That's her log on.

**1:37:36** · All right. So, we got me and Chloe. So, that went pretty smooth. But I want to be able to log in as one of those users on another machine. Right, Chloe?

**1:37:47** · Chloe's going to log in. This is running Windows XP, which I think is going to be totally fine in our setup if I can figure everything out. And so, basically, this thing needs to know that the DNS server is this one. And then, uh, I think that's how it all works. So, my DHCP server, the thing that's handing out IPs to all the machines on the RetroAct network, is a Cisco VXR. And what I'm going to do is go configure that thing to tell everyone that connects that their DNS server is this guy. And let's do that now. And then we'll try to see if the compact can talk to this thing. And we can log in with those usernames. Okay, this is the configuration of that Cisco machine that is operating as my DHCP server. And before you get all mad at me, if you're an expert at this stuff, you will notice I'm excluding the first 100 addresses.

**1:38:40** · And this was a bad idea, but I gave that machine an IP of 10011. I think you should not do that. The Active Directory server needs a static address. In my case, I'm going to be fine. Uh, but I I'll probably clean this up later. I just wanted to note that.

**1:38:57** · But my DHCP pool is called RetroArack and it already has a DNS server because I was messing around with a Sun Micros Systemystems Java station. It's right up there. This will make a comeback. We will get Java OS on this thing. It's running NetBSD right now. Uh, that'll be another video. But we need to go change DNS server so that everyone that connects over DHCP such as the compact laptop and we going to get up again. We are going to configure this thing to reach out on the network and ask the retroact for an IP address and the retroact operating as a DHCP server will also say your DNS server after we change it is the Windows 2000 machine. So, let's get in here and set the DNS server to 100 111. And I'm going to go double check that that is actually the IP of that thing because I can't remember because I was like two days ago. That's the right IP. So, we're good. Let's write out this configuration. So, the next time I restart that router, it will remember this. And it's important to remember what a DNS server is doing. it is translating a human-friendly name into an IP address. So when the compact reaches out for clab retro.local, you can think of that as a any domain name like google.com, it's going to tell it, hey, if you want to go find out who clab retro.local is, you need to go talk to 10011.

**1:40:27** · That's all DNS does at the at the base level. So, it is a sophisticated, complicated way to take something a human understands like google.com or clabretro.com and turn that into the IP address of the server at the end of the day that needs to serve that request.

**1:40:44** · That's all we're doing right here. And now, as is tradition, thanks to Merlin for this one from the serial port Discord, which you get access to if you subscribe to my Patreon and use a DNS server to get from clabretro.com to what I'm showing there. I am going to go find the power supply for this and hope that it still works. This is a weird one. Got this Dell power supply output 19.5 volts which is pretty common and too small on the compact. Our lord and savior the weami. So looking at the back this thing wants 19 volts not 19.5 which the wami will happily provide.

### Controlling Another Computer

**1:41:24** · This thing doesn't start up anymore.

**1:41:29** · Can't remember.

**1:41:32** · It used to work. Let me go review some footage. The last time I used this thing, it doesn't have a hard drive. Can't really remember why I did that. I made a really big mistake.

**1:41:44** · Oh no.

**1:41:49** · Oh man. Well, there is a hard drive in there. It's one of these little guys.

**1:41:53** · And uh I forgot to put it on its tray. I was just seeing if it fit. Now it's stuck. Why did Why did I take the original hard drive out is the real question. New plan. Dell laptop latitude D16 run Windows XP. Maybe that compact never worked. I don't remember. Okay. On this guy, if we go look at the network properties, TCP IP, I'm obtaining IP automatically. Obtain DNS automatically. And if we do an IP config, we can see got this collab retro thing going on. Don't know what that's about. This is 101. It knows its gateway. And if we do IP configall, it has been told that its DNS server is 10011.

**1:42:40** · That is the DL380 G2. And if we ping the DL380 G2, which is the host name, it knows that it's 111 and responds. So, so far so good. And now I've gone to my computer and I clicked properties. I went to computer name and I'm going to click change down here and I'm going to be part of a domain. And it was clab retro.local.

**1:43:07** · Let's see.

**1:43:10** · Uhoh.

**1:43:11** · Let's get some details. DNS name does not exist. I did something wrong. That clab.ro Retro DNS suffix was suspect. So let's do clab retro.lo here. No, I've configured something wrong. I think we need to go back to the Windows 2000 machine and take a look. Yeah. So if we do an NS lookup of clab retro.lo non-existent domain, something's wrong.

**1:43:47** · As much as I would like to crack another one for you, that was a pretty good joke I had going. I'm not even halfway through this one. So, probably for the best. Before we investigate the Windows 2000 machine, let's just see if we can join the Clab Retro domain like that from NetBoy. No way. Oh. Oh, it's asking for the password I set up active directory. So, this is actually administrator.

**1:44:15** · Is this going to work?

**1:44:17** · Come on. Big theme of the night. Having a mouse so you know if Windows is doing something. This is very very very good news. Welcome to the collab retro domain. I think it's going to want us to restart. Yep, I will do that. Is this going to work? Yeah, look at that. It knows the domain. I'm D16. The latitude d16.clabretro. Oh man. Yes. Restart. We got to log on as Chloe.

**1:44:49** · Okay, restarted.

**1:44:52** · And I want to log on to the collab retro domain. And I'm Chloe K.

**1:45:00** · Let's see.

**1:45:02** · Incredible timing. She literally walked right in. Is it going to let us in?

**1:45:06** · Chloe, can you get to work on your spreadsheets or what? Yes.

**1:45:14** · Oh, yeah.

**1:45:17** · setting up personalized settings so you can control everything that the user sees uh from the Windows 2000 machine. Yes, Chloe Kardashian. Yes, Chloe doesn't even care. She's just going to try to eat these parts from the compact machine.

**1:45:34** · Uh side note about that compact machine, I don't think it ever worked. I'm pretty sure I thought it was this one. This one clearly works. I think I've used this one in other videos. Right, Chloe? This one is a success beer. We will not be drinking this in anger. Dude, she is so ready to work on some Excel spreadsheets.

**1:45:56** · Finally accomplished what I set out to do. An active directory server on Windows 2000 and some other Windows machine that can use the users I set up over here to log in with. What a journey. Oh, but we're not done. Let's poke around here and see if like we can see this computer and maybe change something about Khloe's, you know, desktop experience. I mean, me, let's mess around with this thing. So, I'm guessing over here in computers, we should see the DL610.

**1:46:28** · Yes, we can. What can we do with it? We can learn about it. This is pretty impressive. So, Windows 2000 obviously came out before Windows XP. Well, I suppose I've got the later service pack, but wow. So, you can make all sorts of stuff here. So, like I can make a new group and you could group users together and give them certain rights to a shared folder. This thing can act as a SMB share. Like, we we can man, we can just do so much more with this going forward.

**1:47:03** · So, let's go look at Chloe here. And we've got a lot more going on. We can run a script when that user logs in environment. I can control that computer. Maybe that would be something fun to figure out. Had trouble figuring out the remote management thing. So, what we're going to do is change Khloe's default wallpaper to this beautiful Windows 2000 wallpaper. And it is sitting here.

**1:47:32** · Uh, can I show you the full path? Okay, it's in the C drive Winnt web wallpaper. It's a JPEG, so that might be a problem. But what we're going to do is create a new folder in the C drive. And we will just call it share. Paste that bad boy in there. And then we are going to share this thing. And we'll call it share. That's fine.

**1:48:03** · permissions.

**1:48:05** · Everyone has everything.

**1:48:10** · Perfect. Back in Active Directory, we're going to go to properties for my domain. We are going to create a group policy. I'll just call it default or something, I don't know. And then under user configuration, we've got administrative templates, desktop, active desktop, and then we got active desktop wallpaper. Going to enable this. Uh I probably want to give that thing a simpler name. We'll call this win2K.

**1:48:50** · W2K.jpeg.

**1:48:53** · Uh, again, I don't know if this going to work with a JPEG. Let's find out. Uh, this thing went to sleep, so we might need to do a full log out. Yeah, let's do a log off.

**1:49:11** · Getting back in.

**1:49:15** · That of course didn't work at all. I can reach the share though. There's the image. So, this has exciting implications for retro computing in my future. Oh, it's JPG. Having trouble believing that's the only problem. But let's do the stretch wallpaper style. Apply. Do another log off here.

**1:49:46** · Oh, it's it's thinking about it. Yeah, I think it's going to work. You got the first part of it here. You can do it, buddy. You can do it. I'm almost gonna call that good enough. It knows. It knows. That's how it's supposed to be.

**1:49:59** · There it is. Why does it look like crap?

**1:50:03** · Let's do a full restart. How about that?

**1:50:08** · There it is. After a full restart. Yeah, works perfectly. And now it's gone. And now it's messed up. That's a shame. So, these are the trade-offs you make. Very easy to share files on Windows. Very hard to make them do what they should.

**1:50:27** · But I think you get the general idea. I can control the experience that this user has remotely with group policies and a bunch of other stuff. And you can remove favorites from start menu, remove run from start menu. Let's see if we get something simple to work. So, in my start menu, I have a run option. Of course, I can run stuff over here. We got remove run from start menu. Not configured, enabled, or disabled.

**1:51:00** · Okay, let's try that out. For some reason, I logged out and logged back in again to check this run menu start thing, and the desktop background works perfectly. Imagine being the guy that designed this clouds and someone jumping. The early 2000s were so hopeful. The question is I still have run.

**1:51:25** · Oh, but I can't do it. Okay, fair enough. I'll accept that. Contact your system administrator. I like it. I feel like I'm going to have a lot of fun with this setup. And now Chloe can no longer ping my servers. remember this broken compact that wouldn't boot and I tried to slide a hard drive caddy in there.

**1:51:46** · Also, it didn't make sense that the hard drive would solve that problem. It should still be able to post without booting. So, it's got something much more serious going on. Anyway, I super glued a little piece of plastic to this thing. Oh, don't break. Don't break. Oh. Oh, I'm I'm like so close. There's like this little lip from this plastic and the caddy would have solved this problem. Pull that out. Oh, I'm so close.

**1:52:15** · Yes.

**1:52:17** · Inside this thing is one of these. So, this is a laptop style IDE header and these things convert these to M.2. Uh, anyway, I had put that in there without the caddy and couldn't get it out. So, now I can pull that off. This guy's a problem for another day. Obviously, it wasn't hard the hard drive. I for some reason I thought this was working.

**1:52:39** · Probably something to do with the five tall boys. This has been quite a journey to get to this point where I can remotely manage this machine with a Windows 2000. What is it? Advanced server install. And uh it's pretty cool.

**1:52:52** · What the She's back. I was watching some YouTube videos and apparently you can use a Windows 2000 Active Directory to manage like Windows 10 machines. But yeah, it's it's a whole thing and they came up with all this stuff over 20 years ago. Well over 20 years ago. It's pretty impressive. And to get to this point, if you made it this far, we have been on a journey. I had a bunch of fun making this video. I don't know how well it's going to be received. It's almost two hours long and I rambled a lot and went on a lot of side quests, but sometimes that's just how it goes. I mean, I didn't even film all of it, believe it or not. I filmed more than I usually do.

### Outro

**1:53:34** · I had more troubles than I usually do. But yeah, I honestly thought, despite my compact experiences in the past, that it would be 20 minutes to install Windows 2000 on that thing. I thought it would be no big deal. And yeah, that that broken floppy drive really threw me for a loop. And it was definitely broken.

**1:53:49** · So, the drivers I wrote to the floppy discs that I used in the physical drive and turned into a virtual drive were the exact same drivers I slipstreamed into the Windows 2000 ISO, which worked just fine when I did it that way. But I learned a lot. This was like pure home labbing, just like forgetting which laptops work, fighting with a rail system, learning how to slipstream compact smart array drivers into a Windows 2000 ISO. But I won't take up too much more of your time. I had fun. I hope you had fun. If you really want to support the channel, please subscribe. I am on the road to 100,000. We're over 70,000 subscribers at the moment. And of course, I'm on Patreon. I post behind the scenes type videos, the occasional early release. Don't know how early this one's going to be. This one came down to the wire. I thought I was going to be done with this one two days ago. But that's it for the Compact DL 380 Gen 2 running Windows 2000 with the Active Directory service running. Uh, this will make a return. This opens up a bunch of doors for more Windows stuff and to play around with. So, I'm excited. Thanks for watching. I'll catch you in the next video. Chloe's gonna log in for the day.

**1:54:57** · What are you going to work on today?

**1:54:59** · Doesn't AI do everything now?

**1:55:02** · She doesn't want to work. Chloe just had a big snack. They put like a real shrimp in there. And let me tell you, she likes that.
