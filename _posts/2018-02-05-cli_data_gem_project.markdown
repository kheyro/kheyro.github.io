---
layout: post
title:      "CLI Data Gem Project"
date:       2018-02-05 10:44:32 +0000
permalink:  cli_data_gem_project
---


The project was quite interesting, mixing object oriented programming and scraping. I've always found scraping very cool! I was always amazed by the tought of a scraping bot, imagining the bot navigating on the tousands links on the web. It was very popular before the democratization of API.

Being into cryptocurrencies, I naturally thought of building a CLI gem that would scrape the most popular cryptocoin listing, https://coinmarketcap.com/, and display a list and a detailed information of the selected coin.


# Process
1. How to build a gem
2. Architecture of the project
3. Coding the CLI, scraper and the coin object
4. The scraper
5. Building and pushing the gem

# 1. How to build a gem
First, was to understand how to actually build a gem and which files were needed in order to complete the task. The walkthrough video was quite helpful but as we are using the LearnIDE we are a little more limited when we are manipulating files. I used the gem bundler `gem bundler install` and used the command `bundle gem coin_market_cap` to generate all the necessary files and move them back to the root folder, then edit the gemspec files to include my dependencies `add_runtime_dependecy`.


# 2. Architecture of the project
Following previous lessons and the generated folder structures, the executable will be placed in the `bin` folder, all the classes would be included in `lib/coin_market_cap` and at the root of `lib` the main file required by the executable which will require of the necessary gems and classes (from `lib`).

* `lib/cli.rb` is the CLI classes use to display the information and interact with user
* `lib/scraper.rb` includes the class methods to scrape the main page (list) and the detailed page
* `lib/coin.rb` is reponsible of the `coin` object defining attributes, adding attributes


# 3. Coding the CLI, scraper and the coin object
1. Starting by the CLI was for me the logical step to start from, as it would flow back to the lowest level object and give us all the information and data needed from the object.
2. I started by creating "fake" data to be able to interact with the CLI and make sure that the interface was working correctly. Then I coded to the scraper class, where I moved the "fake" data to and created the method classes called by the CLI.
3. Keeping the "fake" data onto the scraper object, I coded the coin object so that it could be instantiated with attributes and included a class which could set attributes to an existing coin object.
4. I felt like it was logical to process it this way and have the architecture designed this way: Coin < Scraper < CLI

# 4. The scraper
The scraper was a bit challenging, I spent a lot of time trying to scrape a specific piece of information, I ended giving up scraping certain information as there was no way of being able to do it consistently with all the coins. Hence, I restricted it to the possible ones.

One sure that I was not sure of is that `self.list` from the scraper class would be the one instantiating the coin class. I chose to do it here because it was just shorter and seemed like a clean way to do it. But I think that to be really strict it could have been coded on the Coin class, so that scraper stayed the "tool" to grab the data and Coin could take care of its own business.


# 5. Building and pushing the gem
To build the gem I had to first create an account on https://rubygems.com/, then use the command `rake build` to build the gem and `rake release` to push it to the repertory. `rake build` is actually a very useful script as it will also tag the and push to git. 

Unfortunately, `rake release` froze and I had to use `gem push` found on the official documentation to push my gem. I also realized that someone already created a gem with the same name (should have checked before hand...) so I updated the name from gemspec, fixed some others issues and updated the gem and successfuly pushed my gem.

