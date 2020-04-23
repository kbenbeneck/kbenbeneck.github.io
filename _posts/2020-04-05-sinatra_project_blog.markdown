---
layout: post
title:      "Sinatra Project Blog"
date:       2020-04-05 18:17:32 -0400
permalink:  sinatra_project_blog
---


I'm Vegan, there I said it. 
Now you're probably thinking, “I can’t live off of tofu,” or “what does this guy even eat?”

Well, if you’ve ever found yourself using color coded containers from Beachbody or something similar, meals are planned and built with combinations of containers. A red container symbolizes protein, yellow for carbs, green for veggies etc… 

I try to eat balanced and nutritious meals with primary food groups in mind. 
I wanted to display some items from my kitchen and try and show how easy it is to eat plant based. I don’t shop anywhere special to maintain this lifestyle either. 

Now, I don’t want to open my digital pantry up to just anyone, so one must sign up and log in to gain access. If we have any trouble at the gates, the error handling will let you know what went wrong on the sign up form. 

Once we’re signed in, click “Create” to begin the two step process of meal construction. Clicking “Create” sends a get request to the “/meals/new” route which contains the form, outlining the meal model. 

After giving the meal a name and choosing a food for each macro, on submit, the form will send the meal via a post request. During this transfer, your choices are compiled into a hash of key value pairs called params.

params
=> {"meal"=>
  {"name"=>"My Favorite",
   "protein"=>"Tempeh",
   "carb"=>"Sweat Potatoes",
   "fat"=>"Avocado",
   "fruit"=>"Salsa",
   "veg"=>"Peppers"}}

Before saving the creation to the Meals database, the params hash is used as an argument to build a new instance of a meal that belongs to the current user. 

            @meal = current_user.meals.build(params[:meal])
            if @meal.save
                redirect "/meals/#{@meal.id}"

Once saved, the meal is given an ID form the database. That allows this meal instance to be called via a dynamic route. This get request will display or show the meal being called. 

<h1>Looks Yummy!</h1>
<ul>
    <li><%=@meal.name%>
    <li><%=@meal.protein%>
    <li><%=@meal.carb%>
    <li><%=@meal.fat%>
    <li><%=@meal.fruit%>
    <li><%=@meal.veg%>
</ul>


From the show page, the user may navigate to all other CRUD functions via links: to edit or delete this meal, to create more meals,  or to view their collection of creations. 



