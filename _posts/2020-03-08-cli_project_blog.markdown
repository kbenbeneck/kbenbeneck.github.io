---
layout: post
title:      "CLI Project Blog"
date:       2020-03-08 19:53:02 -0400
permalink:  cli_project_blog
---


Leading up to the CLI project deadline, my wife and I found ourselves checking the CDC website daily as we had just decided to cancel our Tokyo vacation and were trying to collect refunds from all of our bookings. Some of our refunds were contingent on Japan reaching level 3 status. Each day, we were monitoring the Corona virus as it spread across the world. 
I noticed, from my prior Nokogiri scraping sessions, that the bulk of the data being presented to me on the webpage was nothing short of a list of travel health advisories with links to read more information regarding each issue. A quick right-click and inspect validated such. I was going to make my first application a practical one. I was going to scrape the Centers for Disease Control and Prevention and provide a CLI for users to navigate the traveler’s health advisories.
I realized that each listed advisory followed the same format by providing an issue, destination, last update, a brief summary and an option to read more information. Those were going to be the attributes for my advisory or Notice object to be instantiated with.
After creating a Scraper class, I needed to add nokogiri as a dependency in my gem spec file and require both nokogiri and open-uri in my environment. In my Scraper class, I created the class method “self.get_notices” and used Nokogiri to read or parse the CDC website with
```
doc = Nokogiri::HTML(open("https://wwwnc.cdc.gov/travel/notices"))
```
After mousing over the website with the selector, I found that each advisory had an “li” tag inside of class named “list-block.” Using the querySelector function within the inspection console, EACH “li” contained all the attributes that I was scraping for, so I used the each method in my Scraper class to instantiate a new Notice object. 
```
scraped_notices = doc.css(".list-block li")
scraped_notices.each do |node|
                     
notice = CdcTravelAdvisory::Notice.new(@issue, @last_update, @summary, @more_info_url, @key_points)
```

I then used css selectors to assign each attribute.
```
notice.issue = node.css("a:nth-of-type(2)").text.strip
notice.last_update = node.css("> span.date").text, 
notice.summary = node.css("#summary")[0].text, 
notice.more_info_url = node.css(“a")[2]["href"]
```
In my CLI class I used
```
 puts "CDC Travel Health Notices:"
CdcTravelAdvisory::Scraper.get_notices
@notices = CdcTravelAdvisory::Notice.all
@notices.each.with_index(1) do |notice, i|
   puts "#{i}. #{notice.issue}."
```
And after running my program, a numbered list was displayed just as intended. Now I needed to provide additional information after the user makes a selections. I needed to scrape every “read more info” link and provide additional info upon request. 

To get one level deeper, I needed to use “more_info_url” that was scraped from the main website, which would return something like “/travel/notices/warnings/covid19-in-rran.” I still needed to construct a url to scrape the deeper information. At first I tried 
```
more_info_site = "https://wwwnc.cdc.gov/"
more_info_site << notice.more_info_url
notice.key_points = Nokogiri::HTML(open(more_info_site)).css(".bullet-list li").text
```
This code worked but my programs speed declined dramatically. When my CLI was calling methods to scrape and list the advisories, it was scraping each “more info” link simultaneously ie opening 22 different websites. 

 To fix this, I needed to adjust my CLI class to only scrape more info after a selection from the list is made. To do this, inputing “info” into the CLI would call a method in my Scraper class to get the details. 
Instead of shoveling strings to construct the scriptable url, I tried using string interpolation to complete the url. 
```
def self.get_details(selection)
        doc = Nokogiri::HTML(open("https://wwwnc.cdc.gov/#{selection.more_info_url}"))
        
        selection.key_points = doc.css(".notice li").text.strip
    end
```
The program now works swiftly to report all of the places one shouldn’t travel. 
