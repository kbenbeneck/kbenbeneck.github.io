---
layout: post
title:      "Rails Project Blog"
date:       2020-06-25 03:52:30 +0000
permalink:  rails_project_blog
---


Leading up to project deadline, these few words of text brought a wave of confusion and uncertainty.

You must include at least one class level ActiveRecord scope method. a. Your scope method must be chainable, meaning that you must use ActiveRecord Query methods within it (such as .where and .order) rather than native ruby methods (such as #find_all or #sort).

Attacking this project requirement ended up being way easier than expected.
In my application, a User has many Albums and each album has a genre attribute. As my albums collection grew, I wanted to sort it. I wanted to find all of the albums belonging to my favorite genre. 
As with most landing spots, we’re going to need to write code in our Model, View, Controller and Routes. 

I want my url to be ‘albums/prog_rock’

get 'albums/prog_rock', to: 'albums#prog_rock', as: :prog_rock

And I want to easily get to my prog rock so I quickly use my new helper “as: :prog_rock” and add this link to my nav bar..

<li><%= link_to "Prog Rock", prog_rock_path %></li>

Model

How are we going to find all of the prog rock?
This class method queries the DB 

  scope :prog_rock, -> { where(genre: "Progressive Metal") }
  def self.prog_rock
    where(genre: 'Progressive Metal')
  end

Controller

In our controller, we define the action (being triggered in routes) and use our class method. 
    def prog_rock   
        @albums = Album.prog_rock
    end

View

Again we use our class method and iterate over albums who’s genres are “Progressive Metal.”
Active record associations are returned and the album titles are displayed. 

<% Album.prog_rock.each do |pa| %>
    <%= pa.title%>
<% end %>

The styling leaves much to be desired but I can say that I now know how to build an ActiveRecord scope method. 
