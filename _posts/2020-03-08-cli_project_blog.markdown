---
layout: post
title:      "CLI Project Blog"
date:       2020-03-08 23:53:01 +0000
permalink:  cli_project_blog
---


This project forced me to learn and relearn so much information. The walkthrough provided by Avi and the recorded Zoom sessions proved to be invailuble. 

The premise of this project is simple. Scrape the CDC Travel Health Notice website and use the CLI to navigate and provide more information upon request. 

My absolute favorite "ahah!" moment was then I had to construct the url that provides specific informatiuon about each notice. When scraping the CDC website, I found it difficult to only gather the individual notice url, so I tried to constuct the url. I was only able to gather "/travel/notices/polio-africa." I shoveled that partial url into a variable set equal to the base url of "http://wwwnc.cdc.gov/." 

I spent way too much time trying to use string interpolation and other methods to construct the url.
