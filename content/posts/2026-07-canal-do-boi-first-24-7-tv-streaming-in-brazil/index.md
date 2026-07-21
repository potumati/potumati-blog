+++
title = "Canal do Boi: The First 24/7 TV Channel on Brazilian Internet"
date = 2026-07-21T11:00:00-03:00
description = "How Canal do Boi became the first 24/7 TV channel on the Brazilian internet - and the full technical journey from RealServer over dial-up in 2000 to WebRTC sub-second latency today."
draft = false
slug = "canal-do-boi-first-24-7-tv-streaming-in-brazil"
tags = ["Streaming", "WebRTC", "HLS", "RTMP", "Case Study", "Canal do Boi"]
categories = ["Case Study", "Software Architecture"]
author = "Eduardo Potumati"
[cover]
    image = "cover.jpg"
    alt = "Canal do Boi streaming platform still live in June 2026"
    caption = "Canal do Boi - 25+ years on the air, still streaming 24/7"
    relative = true
+++

I was there, I handled that. I was 14.

My father and I put the first full time TV channel on the Brazilian internet, over dial-up connections.
And if that wasn't enough, it was a RURAL TV CHANNEL! For a rural audience that had barely heard of the internet. In fact, most of them just despised the internet.

*You might not believe it, but at the time, I remember, there wasn't another one. And today, I've spent time looking for any record of something similar happening in Brazil around 2000. I found nothing. As far as I can verify, this was it.*

{{< figure src="2001.png" alt="Website screenshot from 2001" >}}

Looking back, it's funny how I really can remember creating each one of those GIFs. Using Photoshop to create the textures, layers and exporting. Oh man, each byte counted, we needed to reduce how many colors and frames we would export.

After that, we started using Microsoft Gif Animator… or did we use Photoshop later? I can't remember.

Anyway, I'm getting too nostalgic.

Let's go back to the point, the live streaming.

---

## RealServer - The Beginning

Our first streaming platform was RealServer, from RealNetworks.

We captured the signal from a satellite dish, to a capture board and streamed it via RealServer.

{{< figure src="2001-real-player.png" alt="RealPlayer streaming interface, 2001" >}}

We faced 2 main issues, one of them was "solvable".

The bandwidth was poor, even on the server side, we could only support a few viewers, so I created a simple concurrency middleware using ASP. Basically, when a user connected, I registered the connection in a database, when he left, I updated the database.
If the limit was reached, I just asked the user to come back later.

The other one, I had no control over. In 2000, using a dial-up connection, an audience that barely knew how the internet worked needed to download a 24MB program, fill out a form, follow a multi-step installation, and restart their PC. Just to watch TV, just to try to buy a bull.

RealServer was our partner until late 2003, when I got the Windows Media Server that streamed directly to Windows Media Player. Just a few users needed to download Windows Media Player, it usually was there with Windows.

---

## Windows Media Server - Embedded in the Browser

And it looked like a dream, I was able to "embed" the player! The users didn't even need to "open" an external player, they could just watch on the website. It was even compatible with some BlackBerries.

Ok, I opened it on a new window, because the users wanted to keep navigating on the website.

And the website was still evolving, news, auction calendar, debate forum, et cetera.

Later, in 2004, the platform grew significantly. We now had two live streaming channels: Canal do Boi and AgroCanal. Both running 24/7, embedded directly in the browser.

{{< figure src="2005-tvs.png" alt="Canal do Boi and AgroCanal dual-channel interface, 2005" >}}

Looking at that screenshot now, I notice something I didn't think about back then: I hadn't been putting the creator's credit in the footer. The 2005 version was the first time EquipeA appeared there.

At the same time, Adobe Flash started to earn relevance on the internet, you can see it on the screenshot, the "ad banners" look smooth.

{{< figure src="2005.png" alt="Website screenshot from 2005 with Flash banners" >}}

And we can see an MF Rural banner there. A very busy moment in my life, two important projects.

If you haven't read the MF Rural post yet, [click here](/posts/mf-rural-biggest-brazilian-rural-ecommerce/).

Anyway, let's keep our streaming way.

Windows Media Server served us well for some years.
But its protocol (MMS/ASF) had a problem that we can't accept anymore.

A 40-second delay is troublesome when you're trying to buy in an auction.

And MMS/ASF simply couldn't get the latency any lower.

The encoding side evolved as well. Over the years I worked with Windows Media Encoder, Flash Media Encoder, Wirecast, OBS Studio and vMix.

My next step was to start researching a new technology that could improve the delay time.

---

## Flash Media Server - RTMP and Real Latency

RTMP was the solution.

With Flash Media Server we solved 2 issues:

- **Compatibility:** as I said before, Flash was starting to be popular on the internet, and most users already had Flash installed.
- **Latency:** in 2007, 10 seconds of delay was good enough. You're really looking at the animal you're buying.

Besides, with Flash, we had more interactive and beautiful players.

{{< figure src="2005-player.png" alt="Flash-based streaming player" >}}

And at the same time, Canal do Boi launched another 24/7 channel: Novo Canal.

Now we had three online channels running simultaneously. We decided to create a kind of "media center" where users could choose what to watch. And Canal do Boi became **Sistema Canal do Boi** (a system, not just a channel).

{{< figure src="2007.png" alt="Sistema Canal do Boi media center interface, 2007" >}}

In 2008 CanalDoBoi.com became SBA1.com, assuming its role as a system. Leaving the "one channel" mindset. And the company felt the necessity of having a full time team inside, to deal with technology.

And I kept my path, handling streaming and auction platforms, which demanded continuous development from me.

---

## Wowza + HLS - Taming the Apple Problem

I cursed Steve Jobs (just kidding), because Apple boycotted Flash (I don't know if it was a boycott, but it sounded like it to me).

So iPhones didn't play Flash Streaming (RTMP), so I was pushed to find a solution.
I migrated to Wowza Media Server, a software that delivered HLS content.

At the beginning, I struggled to match the low latency of RTMP.
The default delay of HLS is around 40 seconds.

Shortening chunks, reducing caches, improving codecs… well, my goal was achieved.
Getting HLS down to 8–10 seconds was a huge win in 2013.

And as I left SBA with a good relationship, I was called to help improve their streaming.
Between 2014 and 2015 they contracted me to solve the latency issue.
Desktop and Android were able to play RTMP streaming, and iOS wasn't, so their Wowza wasn't well set up to deal with HLS low latency.

And we created a new website.

{{< figure src="2014.png" alt="SBA website with four channels, 2014" >}}

And there I was, dealing with SBA's new website, with four channels now (Canal do Boi, AgroCanal, Novo Canal and ConexãoMS).

On this point, I admit, I made a mistake.
The client asked for a Wordpress website, and I accepted that, I was young.
I went where my know-how didn't go… Wordpress+PHP, I relied on external developers and I didn't deliver the best experience to the client.

They had other versions of websites created by other partners, at some point they changed streaming infrastructure to Embratel, that delivered a consistent signal, but low latency wasn't their focus - auction is a very focused market, it may not be the focus of a billionaire company.
Canal do Boi, despite my previous work, had a 30-second delay again.
And I was still an expert.

I stepped away from SBA again for a couple of years. But I kept watching from the outside.

I saw the opportunity in 2017, and got in touch with the SBA CEO, to show my streaming.
After some in-person meetings, I had a contract with them again.

> 📰 [Canal do Boi atualiza TV ao vivo com baixa latência e compatibilidade mobile - SBA1, junho de 2017](https://sba1.com/noticias/noticia/69300/Canal-do-Boi-atualiza-TV-ao-vivo-com-baixa-latencia-e-compatibilidade-mobile)

---


### AWS EC2 - Scaling for Third Parties

By 2018, the platform had grown beyond Canal do Boi itself. We were providing streaming infrastructure for third-party broadcasts,
including the gubernatorial debate in Mato Grosso do Sul organized by [Jornal Midiamax](https://midiamax.com.br/politica/2018/debate-midiamax-confira-onde-assistir-o-1o-confronto-entre-candidatos-ao-governo-de-ms/), streamed live across all
79 municipalities of the state.

*For that kind of event, a single dedicated server wasn't enough.*

We ran the stream on AWS EC2 with auto-scaling, pne of our firsts real experiences with elastic infrastructure, and a lesson that would prove critical two years later when COVID hit.

---

## WebRTC - Sub-Second Latency

They and I wanted to do better, and other auction platforms had caught up.

We needed ultra-low latency AND consistent compatibility.

To achieve WebRTC at scale, we moved away from Wowza to a media server with native WebRTC support and the clustering architecture we needed.
I'm not sure, but I believe that in 2019 we launched our WebRTC streaming, with **0.5s delay**.

It's less than a digital TV.

{{< figure src="2019.png" alt="WebRTC streaming platform, 2019" >}}

> 📰 [Canal do Boi lança streaming com latência ultrabaixa - SBA1, março de 2020](https://sba1.com/noticias/noticia/69299/Canal-do-Boi-lanca-streaming-com-latencia-ultrabaixa)

---

## COVID - Scaling Under Fire

Then COVID arrived.

Overnight, thousands of people who used to attend auctions in person were suddenly online.
Traffic surged far beyond what our infrastructure had been designed to handle.

Our first thought was to implement a CDN, but WebRTC isn't compatible with CDN.
WebRTC keeps the connection open encrypted full time (PeerConnection/SRTP) and CDN caches static HTTP fragments.

The solution was to implement an **origin-edge clustering architecture**: a load balancer backed by MongoDB, origin nodes receiving the encoder signal, and edge nodes delivering it to clients. Efficient, scalable. Built by us.

But it has its own issues… while HLS just demands bandwidth, WebRTC demands CPU/Threads, mainly because of the handshake DTLS/SRTP.

So when the pandemic spike stabilized, we made a strategic architectural decision: we downscaled back to a high-performance dedicated server. It simplified the stack, dramatically reduced infrastructure costs, and still delivered the sub-second latency our audience needed.

---

## 25 Years

{{< figure src="2026.png" alt="Canal do Boi still live in June 2026" caption="Screenshot taken June 2026. Canal do Boi is still live, 24/7, 25+ years later." >}}

RealServer to WebRTC. Dial-up to sub-second latency.

A rural audience that despised the internet, now bidding on cattle from their phones.

I was 14 when it started. I'm still here.

When we started, convincing someone to watch cattle auctions on the internet felt impossible.
Today, people bid from their phones with less than a second of delay.

The technology changed completely. The audience changed completely.

Canal do Boi is still on the air.
And somehow, so am I.
