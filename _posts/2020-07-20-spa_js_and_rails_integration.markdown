---
layout: post
title:      "SPA JS and Rails Integration"
date:       2020-07-21 00:44:34 +0000
permalink:  spa_js_and_rails_integration
---


Before completing any Javascript related labs, I took a glance at the project requirements and I was immediately thrown a curve with  “asynchronous,” as in, “interactions between client and server must be handled asynchronously.” My first reaction: Why wouldn’t interactions be in sync?”

It turns out, because JS is a single threaded programing language, only one thing can happen at a time. Not all programmable actions take the same amount of time to execute, for instance, a network or server request. This is one of the many factors that allows users to experience waiting for requested content. 

When communicating asynchronously with fetch, actions can essentially be queued up, with an expected return, or promise. Methods can then be called on the promise. This methodology can allow a programmer to write code as though it were synchronous and also limit blankness while loading. 

In short, instead of handling requests one after another, in succession, asynchronous programming allows multiple requests, simultaneously.


![](https://raw.githubusercontent.com/MSLNZ/msl-network/master/docs/_static/sync_vs_async.pnghttp://)
