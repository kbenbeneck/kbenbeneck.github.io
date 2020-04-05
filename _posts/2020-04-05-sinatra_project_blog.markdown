---
layout: post
title:      "Sinatra Project Blog"
date:       2020-04-05 22:17:31 +0000
permalink:  sinatra_project_blog
---


Disclaimer: Throughout the entire Sinatra unit, my elementary school aged kids have been home from school and I have contemplated taking a break from Flatiron countless times. I decided to power through and regretfully I still have work to do. I thought I would submit what I have and highlight the work still to be completed.

The point of this project is to answer the question, “what is a Vegan meal?” A user may sign up, sign in, and create plant-based meals from dropdown lists of ingredients in my fridge and pantry. 

Right out the gate, we have issues where my users controller is failing to handle new users properly. I am still able to sign in, sign out, view and edit meals. Whenever, when I create a new meal, the meal object is without a user id and does not automatically populate the user’s meals list. These issues all arose when I implemented my users controller code. I am going to need to improve the handling of new users and the meals index page.

Additionally, the layout of edit meals is bothersome. Meals are constructed from dropdown selections and when one views the edit page, the current values are not selected, almost guaranteeing that the meal will be edited. I know that there is a selected options with drop down menus, there must be a way to set selected to meal params values. 

With a little bit of time and hopefully guidance, I aim to rectify these issues.
