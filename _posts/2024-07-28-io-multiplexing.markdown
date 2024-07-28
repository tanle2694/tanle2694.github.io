---
layout: post
title:  "I/O Multiplexing"
title2:  "I/O Multiplexing"
date:   2024-07-28 00:20:00
permalink: 2024/07/28/iomultiplexing/
mathjax: true
summary: 
---

On the series of Crypto Trading, we have an input modules "Agent crawlers", It accounbility for listen messages from Exchange such as Binance, CoinmarketCap, .... Each pricing of couple assets can be monitoring by a web socket. 
So that, we will have loads of concurrent websocket need to listening. 
<div class="imgcap">
    <div> 
        <img src="/assets/crypto_trading/io_multiplexing/image.png" align="center">
        <div class="thecap"> Fig 1: Agent Crawlers</div>
    </div>
</div>
