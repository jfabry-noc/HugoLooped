---
title: "Trying T-Mobile Home Internet"
date: 2023-03-19T18:58:39-04:00
draft: false
tags:
  - T-Mobile
  - Home Internet
  - 5G
  - VPN
image: "/2023/t-mobile_wifi_speed.jpeg"
---

For the majority of my life, I've only had one Internet service provider where I lived. That was originally just a single dial-up provider when I was growing up to only having one coaxial cable provider at 4 different apartments in the span of 11 years. That recently changed when I went from having a single provider (coax) where I'm currently at to having **3** providers (coax, 5G, and fiber.)

While the additional options on top of coax became available at the tail end of 2022, I stuck with my coax provider because I was getting a fairly steep discount from them. When that discount was about to end in February of 2023, however, I decided it was time to check out some other options. While my coax service had been (largely) reliable, the non-discounted price was a bit steep for the service offered (200 Mbps down.) This was also the same provider I had for nearly a decade in the previous town I lived in, and an absolutely terrible customer service experience with them left me with a permanent negative attitude toward them.

## 5G

I first decided to try T-Mobile's 5G home service. I use T-Mobile as my cellular provider (I think that's a prerequisite to sign up for their home service), and in my area 5G absolutely screams, regularly pulling down 500 Mbps or more. When I came home and my phone switched from 5G to my home WiFi, it was a pretty sizable speed downgrade.

While I understand that a lot of people are still a bit leery of using a cellular network for their home Internet, I had some previous experience with this in my last job. I worked for a network company, and one of the fun toys for my home lab was a [Cradlepoint IBR900](https://cradlepoint.com/product/endpoints/ibr900/). I basically double-NAT'd my home network to have all of my work-from-home gear sitting behind the Cradlepoint. In the event my terrestrial WAN link was unavailable, the Cradlepoint would fail over to AT&T's LTE network (5G wasn't really available when I got the device in 2019.) I spent a not-insignificant amount of time working from the Cradlepoint during outages, and even on LTE it was a smooth experience. As a result, I had high hopes for 5G.

## Setup

One of the reasons I opted to try T-Mobile first is that there was really no setup involved from an installation standpoint, so if things didn't work out it would be easy to just bail on the service. They sent me a 5G gateway the next day. All I had to do was turn it on, install their mobile app (not exactly [my favorite thing](https://loopednetwork.medium.com/tp-link-ax3000-4cdb961dd761)), and walk through a few steps. Since I viewed this as "kicking the tires" and still had my coax service, my plan was to simply turn off the router connected to my coax modem and make the T-Mobile SSID and PSK the same as what I was currently using so that everything I had would just automatically reconnect to the new gateway. That was a good idea, but for some absurd reason T-Mobile decided to disallow spaces in the SSID. I can't fathom why this would be the case since having spaces in an SSID is complete valid, but there was no means by which to work around it... meaning I had to reconnect all of my devices to a different wireless network to effectively test it. While that's not a huge deal for computers, tablets, and phones, it's truly painful for IoT devices and streaming sticks.

## Performance

Performance was quite good, though admittedly not as good as I was expecting. As mentioned previously, my phone (an iPhone 13) regularly pulls down 500 Mbps or more. My 5G home gateway would usually get around 200 Mbps. I could quite literally set my phone **right** next to the gateway, and while on WiFi I would get 200 Mbps down while swapping my phone to 5G would result in 500 Mbps down.

Here's a speed test I had taken from my phone while connected to WiFi:

![T-Mobile Home speeds](/2023/t-mobile_wifi_speed.jpeg)

And this is an example of relatively _slow_ 5G speeds on my phone itself... which are still significantly better than the home service:

![T-Mobile 5G from iPhone 13](/2023/t-mobile_5g_speed.jpeg)

I'm guessing there must be some kind of traffic shaping that's prioritizing mobile throughput over home throughput, but that's just wild speculation on my part. Living by myself, 200 Mbps is more than enough, though, and was comparable to my coax service that I was paying more for.

I had been more concerned about latency and jitter, but those proved to be non-issues. Conference calls on the web, including watching screenshares and sharing my own screen, worked without any issues. RDP and SSH sessions across the Internet were similarly as performant as I would expect. Streaming video, music, and even playing online games even had no issues.

## Problems

The problem I eventually ran into, though, was that I was struggling to maintain a consistent connection to a VPN I use to access a development environment for work. How frequently I need to be connected to this VPN varies based upon what I'm working on at any given moment, but when I need to access the VPN, I **need** to access the VPN. The behavior I saw is that it would work for some variable amount of time after connecting; sometimes it would be an hour, sometimes it would be an entire day. But occasionally it would suddenly begin to drop 50% or more of the packets, and response times would be measured in _tens of seconds_ rather than milliseconds. This would persist for anywhere from 10 minutes to an hour, at which point it would usually clear up. From there it would again continue to work for a variable amount of time before causing havoc again.

When the issue transpired, **only** the VPN was impacted. Connectivity to the public web was fine, as was throughput, latency, and jitter. Troubleshooting this was fairly easy since when the issue occurred, just trying to `ping` the public-facing gateway for the VPN (not even being connected to it) would show dropped packets and latency. I saw the same behavior when I connected my laptop to my phone's WiFi hotspot, which was naturally also using T-Mobile's network. Then I connected to a VPS I have via SSH and attempted to `ping` the same VPN gateway. This would show no packet loss and 40 ms response times. Likewise, Slack wasn't blowing up with people reporting VPN issues, so clearly I was the only one experiencing a problem.

## Support

After this, I called up T-Mobile to ask them what was going on. The short of it, which I'm honestly a bit impressed that they were very upfront about, was that corporate VPNs often don't work reliably on their network. The customer service representative I spoke to basically said there wasn't anything they could do from their end, and that if this was going to be a problem it would be best for me to use a different service.

On one hand, I really appreciate the candor. On the other hand, if this is such a known issue then I think maybe some sort of disclaimer when signing up for the service would be warranted, such as noting that corporate VPNs may not be usable. If nothing else, I would've tested this facet much more rigorously out of the gate. Even the CSR said that when she works from home **for T-Mobile** she can't use their home Internet service due to VPN-related issues. If nothing else, she offered to make notes on my account so that if I needed to cancel after procuring another service they wouldn't try to hassle me into staying subscribed to a service that wouldn't work for me.

## Aftermath

Almost as soon as that call was over I reached out to the local provider which had recently lit up fiber services in my area and got an appointment for later that week. There was already a drop in my apartment, but I would still need a strand run from that to wherever I would actually egress. Pricing was fairly aggressive, especially because--as a new service in the area--they were offering some special deals that I was able to take advantage of. Once the service was connected and I verified that everything worked, I called T-Mobile back.

I have to give the original CSR credit, as when I called back and explained why I needed to cancel my service, the new CSR didn't try to talk me into staying. She prompted processed the cancellation and provided me with a shipping label to send the 5G gateway back.

## Conclusion

On the whole, I'm honestly a little bit bummed that T-Mobile's service didn't work out for me as I really liked the idea of it. The price was good and--VPN connectivity aside--the performance was great. Humorously enough, just a few weeks after I cancelled my service, I found out that my work is decommissioning the VPN I had problems with as we move our development lab to a new environment. That's not to say T-Mobile's service wouldn't have the same problems with the VPN for the new environment, but it's just amusing timing to me.

In the future, I'd absolutely consider T-Mobile's home service again if I find myself shopping around. I'd probably just try to spend some time working from my phone's hotspot--at least a few days--to see if there are any obvious issues prior to opting in, though.
