---
layout: post
title:      "Sinatra project"
date:       2018-02-23 18:00:01 +0000
permalink:  sinatra_project
---


The project actually took longer than I thought, made few mistakes along the way and I took some time to research and try to do thing the right way. So I started by choosing the subject, which is a app where users can signup, login and add their recipes. I started by setting up the project, views, models, controllers, rakefile, gem, config... 

* Each recipe is composed of a cooking time, a description, a name and a list of ingredients. 
* The ingredients have a name, and in the recipe you can define the quantity and the unit
* There is two type of users, admin (role_id = 1) and user (role_id = 2)
* Only admin user role can access the ingredients list and manage it

## The files architecture


## The Tables

There are 4 tables, I didn't create an "unit" table as I thought it was not necessary, same for "role":

Pictures


### Relationship between tables 
* users `has_many` recipes
* users `has_many` ingredients, through recipes
* users' `username` is unique and can't be empty and users `has_secure_password`
* recipes `belons_to` users
* recipes `has_many` quantities
* recipes `has_many` ingredients, through quantities
* recipes' name is unique, not null
* quantities `belongs_to` recipe
* quantities `belongs_to` ingredient
* ingredients `has_many` quantities
* ingredients `has_many` recipes through quantities
* ingredients' name are unique and not null


## The Routes
Routes are sumed up in the table below


## Tought on the project

The project was not too difficult due to all the exercices that we had to do leading to this project, but I wanted to add an extra layer of difficulty to challenge a little bit myself. And I was stuck at some point, `tux` and `pry` helped me a lot in those times. Can't thank them enough!

## Issues encountered

Those are the mains issues that I came accross during the project

### Many-to-many issue

I first defined `quantities` table a many to many relationship between recipes and ingredients, and started coding and I hit a wall when I add to code the controller which added a recipe, because the quantities tables actually has others fields other than the two foreign keys. Actually, `quantities` which was previously called (`recipe_ingredients`) had a one to many relationship with recipes and ingredients. I had to change the table names, modify code in controllers and models.

### Multiple object within one form

As you can see, the recipe can have multiples ingredients which are also defined by their name, a quantity and an unit. I had to dig a lot to find the value of the `name` attribute of the input element, which had to return an array of hashes that I could then pass to the `create` method of the ingredient model. It turned out that a simple `quantity[][ingredient_id]` did the trick, I then had to remove the element that were not filled.

### Cascade delete

When deleting a recipe, the rows in the table quantities weren't deleted. I had to add the option `:dependent`

` has_many  :quantities, :dependent => :destroy`




