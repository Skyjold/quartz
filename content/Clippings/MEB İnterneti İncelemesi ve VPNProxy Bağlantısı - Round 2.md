---
publish: true
title: MEB İnterneti İncelemesi ve VPN/Proxy Bağlantısı - Round 2
description: Reddit is where millions of people gather for conversations about the things they care about, in over 100,000 subreddit communities.
created: 2025-11-04
modified: 2026-02-08T16:48:26.614+03:00
published: 2020-12-09
tags:
  - clippings
  - dns
---

Esenlikler. [Önceki post](https://www.reddit.com/r/Turkey/comments/efknjd/)'umda MEB internetini incelemiş ve VPN kurulumundan bahsetmiştim. Zamanla post'un yetersiz kaldığını ve yanlış bilgiler barındırdığını farkettim.

Bu postta herkesin kullanabileceği şekilde hazır bağlantı paylaşacağım. MEB ağı ve kullandığım program olan [v2ray](https://www.v2fly.org/en_US/)'in nasıl çalıştığına dair inceleme de bulunacak.

---

# Hazır Bağlantı

## Windows

[v2rayN-core](https://github.com/2dust/v2rayN/releases) (Çince açılıyor, uygulamayı açınca çember içindeki soru işaretini seçip İngilizceyi seçip uygulamayı kapatıp açabilirsiniz. Yönetici izni istemez, akıllı tahtalarda kolayca kullanılabilir.) Bunu kopyalayın:

> yanlış yerdesin, en üstteki linke bak

Servers -> Import bulk URL from clipboard

Sağ altta v2rayN simgesine sağ tıklayıp en üstte http proxy'e gelip global mode'a tıklayın. Proxy sistemde aktif olacaktır.

## Android

[v2rayNG](https://github.com/2dust/v2rayNG/releases) - Bu QR Kodu ekleyin.

## iOS

[Shadowrocket](https://apps.apple.com/us/app/shadowrocket/id932747118) (Paralı uygulama, [bu sitede](https://free.shadowrocket.online/) bulunan herhangi bir Apple ID ile giriş yapıp ücretsiz indirebilirsiniz.)

Bu QR Kodu ekleyin. Sonra "Global Routing" seçeneğini "Proxy" olarak seçin.

## Linux & macOS

[Qv2ray](https://github.com/Qv2ray/Qv2ray/releases) - Bu kodu ekleyin:

> yanlış yerdesin, en üstteki linke bak

---

# MEB İnterneti İncelemesi

MEB ve TurkTelekom arasında yapılan anlaşmada her okula verilen 200Mbps simetrik hız bu yıl içerisinde yerini 50Mbps simetrik hıza bıraktı.

Sadece TCP üzerinden HTTP/HTTPS protokollerine erişim açık.

Bu protokollere sadece port 80 ve 443 üzerinden erişim açık.

Domain isimleri filtreleniyor. Bunun için FortiGuard'ın website kategorilendirme çözümü kullanılıyor. İstenilmeyen kategori veya kategorilendirilmemiş bir domain'e erişim engelleniyor.

Bazı IP adresleri hariç domain adı olmadan IP adreslerine doğrudan bağlantı engelleniyor.

## Veri Trafiği Güvenliği

MEB'in ağında [SSL Inspection](https://kb.fortinet.com/kb/documentLink.do?externalID=FD46282) olduğundan yapılan TLS/SSL bağlantıları güvenli değil. Bu yüzden HTTPS kullanan tüm internet sitelerinde "bu site güvenli değil" uyarısı çıkıyor. Uyarıları kaldırmak için root sertifikası kuruluyor.

İnternet trafiğinin gizlenmesi için tünelin kendine özel bir şifreleme yöntemi kullanması şart.

---

# v2ray İncelemesi

[v2ray](https://www.v2fly.org/en_US/) websocket üzerinden TCP ve UDP paketlerini gönderen bir programdır. Websocket hakkında bilgilendirici bir alıntı:

WebSockets are an extension to the HTTP protocol which upgrade a HTTP connection so that data can be exchanged bidirectionally, rather than in the traditional request/response pattern.

v2ray İran, Mısır, Çin ve bazı belediyelerde açık Wi-Fi'larda uygulanan kısıtlamaları aşmada da kullanılabilecek bir çözümdür.

## Engel Aşımı Nasıl Çalışıyor

v2ray ile sunucumuza istediğimiz domain adı (custom HTTP Host Header & SNI Server Name) ile bağlantı yapabildiğimizden yasaklı olmayan bir domaini girip sunucumuza kolayca bağlanabiliyoruz.

Bu yöntem yüzünden Cloudflare gibi proxy hizmeti veren hizmetleri kullanmamız mümkün olmuyor. Çünkü Cloudflare sunucularına gönderilecek olan domain adı (örnek youtube.com) kendi domain adımız ile uyuşmadığından bizim sunucumuza yönlendirilemez.

v2ray'in vmess protokolünde şifreleme yöntemi bulunduğundan v2ray kullanarak veri trafiği gizlenebilir.

## Potansiyel Engelleme Teşebbüsleri

Kendi domanimizi kullanmadığımızdan dolayı domain adının engellenmesi sorunu ortadan kalkıyor.

MEB tarafından realistik olarak yapılabilecek tek şey sunucu IP adresini engellemek olacaktır. Fakat veri trafiği, engelli olmayan bir domain'den geçtiği için (örnek youtube.com) bağlantının engel aşma amaçlı olduğunun tespit edilmesi zorlaşacaktır.

## Kendi Sunucunuzu Kurun

[sunucu.com](https://sunucu.com/)'dan VPS kiralamanızı öneririm. Sunucu Türkiyede bulunduğundan gecikmesi düşük oluyor. Ayrıca engelsiz internet sağlıyorlar. v2ray kurulum detayları aşağıdadır.

---

# Detaylı Bilgi

Bu postta kısaca anlattığım MEB ağı üzerinde yaptığım araştırmalara, oluşturduğum [knowledgebase](https://www.notion.so/MEB-ISPs-44cf87be16614735af0f2097a2e9b096)'den ulaşabilirsiniz.

v2ray hız testi, kendi sunucunuzu kurma, ve v2ray hakkında daha fazla bilgiye oluşturduğum [bir diğer knowledgebase](https://www.notion.so/v2ray-78f64389a9b4454c8b8d7e887a8394cb)'den ulaşabilirsiniz.

v2ray üzerinden WireGuard/OpenVPN bağlantısı yapmak isteyenler v2ray knowledgebase'te "Configurations" sayfasında "UDP/TCP Port Mapping" bölümüne bakabilir.

---

22/12/2020 Güncelleme: Bağlantı, HTTP/2 üzerinden gidecek şekilde güncellendi.

01/01/2021 Güncelleme: Bağlantı, yeni sunucu IP adresi ile güncellendi.

---

## Comments

> **AutoModerator** • [1 points](https://reddit.com/r/Turkey/comments/k9dc50/comment/i83bz64/) •
>
> Please report any rule violation. ([Rules](https://www.reddit.com/r/Turkey/about/rules/) and their [details](https://www.reddit.com/r/Turkey/wiki/rules))
>
> - Memes are not allowed here, use [r/TurkeyJerky](https://www.reddit.com/r/TurkeyJerky/) for memes.
> - All posts must have a source and their titles must be descriptive.
> - Shitposts and meta discussions(posts about other subs) are not allowed.
> - News articles must be submitted with a link. If not submitted as a link, the link must be added to the comments. Posts with just a title and screenshot will be removed.
>
> _I am a bot, and this action was performed automatically. Please_ [_contact the moderators of this subreddit_](https://www.reddit.com/message/compose/?to=/r/Turkey) _if you have any questions or concerns._

> **\[deleted]** • [3 points](https://reddit.com/r/Turkey/comments/k9dc50/comment/gf3q7x7/) •
>
> Yeenim bununla borno izleyebiliyoğmu?
