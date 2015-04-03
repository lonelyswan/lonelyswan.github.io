---
layout: post
title: 用布隆过滤器给大文件去重
category: algorithm
description: "大文件去重，这种事情往往会让人烦恼，想在合理的内存空间内，合理的利用CPU，那么布隆过滤器不失为一种不错的选择..."
modified: 2015-03-28
tags: [algorithm]
image:
    background: shattered.png
---

大文件去重这种问题，记得以前校招面试的时候曾经遇到过，基本思路无外乎分而治之，hash一下等等，布隆过滤器了解的也不多，只能说是听说过这等神奇。今天来看看给大文件去重上，他能发挥多大力量吧。



