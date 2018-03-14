---
layout: post
title:      "Rails project: Cryptofolio"
date:       2018-03-14 10:21:09 -0400
permalink:  rails_project_cryptofolio
---

## Description

Cryptofolio is an app that let you create an manage multiple crypto portfolio. It was not too complicated to code, but I had I had to go through the course again to get a reminder of some notion that came ealier in the Rails section. User can login with facebook or just sign up through a common form. All the coin are loaded and saved in the database through an API made available from [coinmarketcap](http://coinmarketcap.com) website (thanks guys!). The admin user is loaded from the seed file and it has access to the list of coin, and can click on a button to refresh the list of coins. It was pretty fun to code it.


## Tables

![http://media.deniscodes.com/03_rails/tables.png](http://media.deniscodes.com/03_rails/tables.png)

The app has 4 tables:
* Portfolios
* Coins
* Users
* Transactions

Relationship
* Coins `has_many` transactions, and `has_many` portfolios through transactions
* User `has_many` portfolios
* Transacions `belongs_to` coin and to porfolio
* Portfolios `has_many` transactions, and `has_many` coins through transactions

## Design

I made some sketches of the way the app should look (it is not complete):


![http://media.deniscodes.com/03_rails/design.png](http://media.deniscodes.com/03_rails/design.png)

## Gems

I added bootstrap 4 to style my app faster, and httparty was used to parse and call the API provided by coinmarketcap.


## Main issues

* Not really an issue, but the longest part was maybe to code those calculation and the stats, grabe the right data and combine them to have the right info. 
* Management of date type of data, I always had hard time choosing between storing timestamp or datetime and displaying it in the right format. Moreover, I had some issue to populate the date_field from the form helper as it had to follow the right format. The partial from the date input, being able to populate it with the right value took a little time to figure out.
* I originally started without the Portfolio tables thinking that I would be able to create a nested attribues for one of the model, but it just didn't make sense. So I ended up re-coding many things to have a portfolio model that fitted in my app. Hopefully everything went well.


## Conclusion

I am very satisfied of the app, lot of features can be added so that it would be much nicer to use, such as nice charts, importing data from wallet, etc... Rails is very comfortable to work with, but the documentation is sometimes a little confusing. Rails, sinatra, activerecord, version of ruby etc...
