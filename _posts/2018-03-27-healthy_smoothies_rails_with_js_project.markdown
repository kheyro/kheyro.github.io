---
layout: post
title:      "Healthy Smoothies! Rails with JS Project"
date:       2018-03-27 19:17:36 +0000
permalink:  healthy_smoothies_rails_with_js_project
---


## Description

I am a big fan of smoothies! It’s healthy (depending on what you blend, of course) and most importantly, it makes you **save time** by eating a multitude of fruits at **ONCE**! I usually browse smoothie’s website to look for fresh and new recipes, but this time I decided to create my own. It is quite simple, and very close to the recipe type of website. User can manage their smoothies’ recipes and decide to share it with the community (make it public) if they want to. An user can sign up/sign in with Facebook.


## Tables

The app has 5 tables:

* Users
* Categories
* Smoothies
* Quantities
* Ingredients


## Relationship

* ​Users `has_many` smoothies
* Categories `belongs_to` smoothie
* Smoothies `belongs_to` user, `has_many` quantities and `has_many` ingredients `through` quantities
* Quantities `belongs_to` smoothie and `belongs_to` ingredient
* Ingredient `has_many` quantities

![tables](http://media.deniscodes.com/04_rails_with_js/tables.png)


## Mockups

I drew a simple mockup of the smoothies’s card list and few buttons, just to have a general idea of how it should look like.

![mockup](http://media.deniscodes.com/04_rails_with_js/design.jpg)


## Gems

Gem used: omniauth, omniauth facebook, dotenv, bootstrap, jquery-rails, active_model_serializers and faker


## Challenging part


### Wrong singular / plural

It took me some time to figure out, but rails (or ruby?) kept looking for a controller named `smoothy` (supposedly singular of `smoothies`), even though I clearly stated all the relationships in the model using `smoothie`. I had no choice but to use `smoothy`, quite annoying as I would naturally write `smoothie` for my variables, function names etc...


### Active model serializers with associations with JSON API

For the app, I tried as much as I could to stay JSON API 1.0 compliant, which involves setting up the JSON API initializer for AMS.

```ruby
ActiveModelSerializers.config.tap do |c|
  c.adapter = :json_api
  c.jsonapi_include_toplevel_object = true
  c.jsonapi_version = "1.0"
end
```

It took me a lot of time to figure out how to properly pass association down to the JSON object, passing for example the user `name`, the ingredient `name` and its `quantity`. `belongs_to` and `has_many` are not taken into account while using Json Api standard. The jsonapi is very specific and association should be passed through `included` key and `relationships` key, as specified here : http://jsonapi.org/format/#fetching-relationships and here http://jsonapi.org/format/#fetching-relationships. The solution was to include the desired object in the render declaration.

`render json: @smoothie, include: [:user, :quantities]`

As it is also very difficult to deal with `included` key for several associations, I had to customize the `smoothy` serializer attribute to include the association in the key `data` json response, easier to deal with compare to `included` which is not grouped by any key. 

To customize an attributes, declare the attributes and create a method, as follow:

```ruby
def category
    CategorySerializer.new(object.category, root: false)
  end
```


### ActionView Disabled submit button 

Somehow Action View disable any `button` after it is clicked (if it is part of a `form`). I first thought that it was due to Bootstrap JS, but after searching for some time I found out that it was from Ruby/Rails. To disable the functionality, simply add this line during initialisation:

`Rails.application.config.action_view.automatically_disable_submit_tag = false`


### link_to not firing/loading JS action

For some reason, any link generated using the ActionView Helper `link_to` didn’t load the JS files scripts or files on the targeted page. Adding a listener on `turbolink` fixed the issue:

```javascript
let ready = () => {}
$(document).ready(ready);
$(document).on('turbolinks:load', ready);
```

## Conclusion

This was an very interesting and fun project to work on. It definitely took more time than expected. It always ended in a mess with JQuery, it is definitely a very (maybe too) flexible language, but always fun to play with. Working in two different stacks with Ajax is also always very challenging when it comes to debugging in the right stack or at the right place. To conclude, AMS is a powerful tool to JSON, but when it comes to customizing it the right way, it takes a lot of reading and effort.# Enter your title here

The content of your blog post goes here.
