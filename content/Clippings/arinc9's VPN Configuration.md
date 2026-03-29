---
publish: true
title: arinc9's VPN Configuration
description: A tool that connects everyday work into one space. It gives you and your teams AI tools—search, writing, note-taking—inside an all-in-one, flexible workspace.
created: 2025-11-04
modified: 2026-02-08T17:12:25.992+03:00
published: 2026-02-08T17:12:25.992+03:00
tags:
  - clippings
  - dns
  - vpn
  - dpi
  - todo
---

### Caddy Configuration

/etc/caddy/caddy.json

{ "apps":{ "http":{ "servers":{ "srv0":{ "listen":\[":443"],"routes":\[{ "handle":\[{ "handler":"subroute","routes":\[{ "handle":\[{ "handler":"reverse\_proxy","upstreams":\[{ "dial":"127.0.0.1:1923" }] }],"match":\[{ "path":\["/vmess-ws-public"] }] }] }],"terminal":true }],"tls\_connection\_policies":\[{ "certificate\_selection":{ "any\_tag":\["cert0"] } }] },"srv1":{ "listen":\[":80"],"routes":\[{ "handle":\[{ "handler":"subroute","routes":\[{ "handle":\[{ "handler":"reverse\_proxy","upstreams":\[{ "dial":"127.0.0.1:1923" }] }],"match":\[{ "path":\["/vmess-ws-public"] }] }] }],"terminal":true }] } } },"tls":{ "certificates":{ "load\_files":\[{ "certificate":"/etc/caddy/self-signed.crt","key":"/etc/caddy/self-signed.key","tags":\["cert0"] }] } } } }

### nftables Configuration

Enable start at boot:

sudo systemctl enable nftables

/etc/nftables.conf

table ip arinc9-vpn { chain prerouting\_dstnat { type nat hook prerouting priority dstnat; policy accept; ip daddr 149.91.1.15 iifname "ens18" udp dport 123 counter dnat to 162.159.192.1:2408 } chain postrouting\_srcnat { type nat hook postrouting priority srcnat; policy accept; oifname "ens18" counter masquerade } }

### systemd-resolved Configuration

/etc/systemd/resolved.conf

\[Resolve] DNS=208.67.222.222#dns.opendns.com FallbackDNS=208.67.220.220#dns.opendns.com DNSSEC=no DNSOverTLS=yes

### sing-box Configuration

Enable start at boot:

systemctl enable sing-box@public

/etc/sing-box/public.json

{ "inbounds":\[{ "type":"vmess","listen":"127.0.0.1","listen\_port":1923,"users":\[{ "uuid":"6be3e1b2-05e1-46a1-ad36-70aaabaa8d12" }],"transport":{ "type":"ws","path":"/vmess-ws-public" } }],"outbounds":\[{ "type":"direct","tag":"direct" }],"route":{ "rules":\[{ "ip\_cidr":"127.0.0.53/32","port":53,"action":"route","outbound":"direct" },{ "ip\_is\_private":true,"action":"reject","method":"drop" }] } }
