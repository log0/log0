---
id: 268
title: 'Summary: High Performance Browser Networking'
date: 2013-12-29T22:45:19+00:00
author: lo
layout: post
guid: http://www.chioka.in/?p=268
permalink: /summary-high-performance-browser-networking/
categories:
  - Summaries
tags:
  - Browser Networking
  - Summary
---
<p style="text-align: center;">
  <img class="aligncenter" alt="" src="http://orm-other.s3.amazonaws.com/hpbnsplash/hpbncover.jpg" width="360" height="472" />
</p>

[edit: 12/30/2013 &#8211; Chapter 5 and 6 added.]

<span style="line-height: 1.714285714; font-size: 1rem;">Earlier there is the &#8220;</span><a style="line-height: 1.714285714; font-size: 1rem;" href="http://chimera.labs.oreilly.com/books/1230000000545">High Performance Browser Networking</a><span style="line-height: 1.714285714; font-size: 1rem;">&#8221; book written by Ilya Grigorik, which is a really great book which goes into some details but not too low, but high level enough to give you a really good understanding on the whole stack. I highly recommend this book. Below I will post my chapter summaries as I read.</span>

## Chapter 1 &#8211; Primer on Latency and Bandwidth

  * Bandwidth is the throughput of a data link.
  * Latency is the time to send something from A to B.
  * Latency can be observed consciously if there is more than 300ms of delay.
  * Latency is the sum of propagation delay (time from s to t), transmission delay (time to push the packet into the link, depends on link bandwidth and packet size), processing delay (time for router to read packets, etc), and queuing delay (time until packet can be processed at router).
  * We can improve bandwidth by adding more fibers, more links, etc. Latency, not as easy due to physical limitations (an optic fiber allows ~1.5 of speed of light, hard to get more). One way is to put servers closer to users (CDN).

## Chapter 2 &#8211; Building Blocks of TCP

  * TCP three-way handshake is expensive. If 300ms is the limit, distance from HK to California = 8031.12 miles means 43ms, rtt 86ms. That&#8217;s ~1/3 buffer gone. TCP Fast Open (TFO) tries to solve this by sending data in initial handshake.
  * TCP flow control solves the issue of overwhelmed receiver. It works by receiver advertising to the sender the number of bytes to transmit in order not to overwhelm the receiver (max = 65535 bytes). Very small, now we have TCP window scaling (65535 bytes &#8211; 1GB bytes), communicated at start of TCP handshake.
  * TCP slow start solves the issue of network congestion. It works by server sending just n := 4 packets max to the client, after client ACKs all 4 packets, sends n := 2*n of the packets. Backs off by half ( n := n/2 ) on packet loss. This resolved network congestion, but introduced high lag especially over short bursty TCP connections. (Think why we pipeline CSS in webs!). (Note: Since April 2013 RFC 6928, n := 10 at start.)
  * On packet loss, TCP reduces amount of inflight data by decreasing congestion window size (sender-side limit). Traditionally uses the AIMD (Additive Increase, Multiplicative Decrease), but too inefficient (above). Linux 3.2+ uses PRR (Proportional Rate Reduction), an improvement from the past (TCP Tahoe and TCP Reno) .
  * Head-of-line blocking: Suppose packets {1,2,3,4} are sent, but {1} is dropped, {2,3,4} has to be kept in buffer and cannot be processed until {1} arrives. To high level applications, packet always arrive in orders. But, now we have extra delays. In some applications such as Skype, we don&#8217;t need in-packet properties, (or other TCP guarantees) missing some audio packets is OK, we can just blank out the gap rather than pausing the whole video conversation. Know when to pick TCP or UDP.
  * In general, use linux kernel 3.2+, increase congestion window size (=10), enable window-scaling, put servers near users, use as few TCP connections as possible (pipelining) are good advice.

## Chapter 3 &#8211; Building Blocks of UDP

  * UDP is loosely defined as everything that TCP isn&#8217;t. UDP has 1) no guarantee on delivery 2) no guarantee on packet order 3) no state 4) no congestion control. Application developers have to build the features you need.
  * NAT translators maintains a translation table of what the public (ip, port) maps to internal (ip, port) for successful routing of packets. But when should NAT translator expire a NAT record? In practice, routers nowadays drops connections after a period of time. To prevent expiration, applications have to send stay-alive packets to keep the record in translation table alive.
  * Inbound UDP is difficult. E.g. Google Talk is a P2P protocol based on UDP, but what if the clients are hidden behind NATs of NATs of NATs? NAT traversal algorithm to the rescue : STUN, TURN and ICE.
  * In general, building on top of UDP: design for the features you need selectively in TCP (add sequence id if you need ordering, etc), use keep alives due to unexpected NAT record expiration, don&#8217;t fragment your UDP (size < MTU).

## Chapter 4 &#8211; Transport Layer Security (TLS)

  * TLS incurs heavy latency cost &#8211; 2 RTT.
  * With TCP 3-way-handshake, 3 RTT.
  * Browsers that open new TCP connection for each resource could have latency.
  * In practice, 1) servers use TLS resumption to reduce 1 RTT, 2) a proxy server is setup near client, where client only suffers the RTT with the proxy server.
  * Proxy server pools long-living SSL tunnels with the web server. Usable even for non-cacheable data. 3) Session caching, re-use the TLS so client does not need to re-negotiate in 3 RTT time.
  * Monitor your session hit rates.
  * TLS adds extra items (certificate list, headers, etc) to packet, so limit size of TLS packet helps decreases packet fragmentation.
  * TLS builds on TCP, and must require all packets to be present to be processed, so if one packet dropped, unpredictable delays could happen.
  * Always upgrade your TLS libraries to get bug fixes and significant performance improvements.

## Chapter 5 &#8211; Introduction to Wireless Networks

  * **Many widespread wireless technologies.** There are tons of widespread wireless technologies: WiFi, Bluetooth, ZigBee, BFC, WiMAX, LTE, HSPA, EV-DO, satellite services, etc. Most of them start with one purpose but evolved. e.g. Bluetooth have seamless interoperability with WiFi now (i.e. +HS).
  * **Maximum channel capacity model.** All wireless technology have a maximum channel capacity governed by a mathematical equation (wiki: Shannon, Channel Capacity).
  * Electromagnetic waves. All wireless technology run over electromagnetic waves. This is a scarce resource you see companies fight for on news.
  * **Different frequencies offer different performance.** Not all frequencies of the spectrum offer the same performance. Low-freq travel further and cover large areas, but needs bigger antennae and more competition. High-freq travel less far but more data, but need more infrastructure.
  * **Subject to constant background noise.** There is always background noise, the further you are away from the sender, and the more clients compete for bandwidth, the poorer the performance you will have. Think of it 1 pair of people talking on the phone and 3 pairs of people talking on the phone.
  * **Modulation.** The digital data has to go analog signal to be transferred. Different encodings affects robustness to noise and interference and the amount of data transfer.

## Chapter 6 &#8211; WiFi

  * **No guarantee on throughput or latency.** WiFi does not have any central scheduler, so there is no way to make guarantees. You may have QoS (Quality-of-service) for a WiFi network, but overlapping WiFi networks can compete with each other too.
  * **WiFi has a limit of bandwidth of 2.4 GHz and also newer 5 GHz bands.** The b and g standards are the most deployed nowadays, operates in the 2.4 GHz band. The ac standards operates in the 5 GHz only. The n standard operates in both.
  * **WiFi is subjected to noise like general wireless technology. **
  * **WiFi competes with other wireless technology for bandwidth.**

&#8230;more to come!