---
layout: post
title:      "Workout Planner: React Redux Portfolio Project"
date:       2018-04-04 10:31:14 -0400
permalink:  workout_planner_react_redux_portfolio_project
---


## Introduction

For this React Redux Rails project I have decided to code a workout planner, which could be very handy and useful. I always wished I had a simple workout planner along with me during my workout session. It doesn't integrate all the functions that I would need but it is a beginning. It is always important to keep track of your progress, it is a real source of motivation. So I naturally thought of coding my own workout planner! 

The app allows you to create program, and assign several workout to it. A workout is composed of an exercise, a number of sets and a number of repetitions.


## Tables

The app has 3 tables:
* Programs
* Exercises
* Workouts


## Relationships

* Programs `has_many` workouts, and `has_many` exercises `through` workouts
* Exercises `has_many` workouts, and `has_many` programs `through` workouts
* Workouts `belongs_to` Exercise and `belons_to` Programs


![tables](http://media.deniscodes.com/05_react_redux_rails/tables.png)



## Mockup

Here is a simple mockup of the Program list view

![mockup](http://media.deniscodes.com/05_react_redux_rails/design.jpg)


## Gem and JS library

* GEMS: active_model_serializers and foreman (to boot both server using customized rake command)
* JS LIBRARIES: isomorphic-fetch and bootstrap


## Chanllenging part


### ComponentWillReceiveProps

For some component, we sometimes load data from the lifecycle function `componentDidMount`. It works great on first load, but somethimes when the component is loaded via a `Link` component, react doesn't refresh the view, in particular it doesn't call componentDidMount again. Because the component is already mounted, even if the url changes, the only way to "force" the update is to use `componentWillReceiveProps` and test for example `nextProps.match.params`. If the the test passes then dispatch and action.



### React + Redux in general

#### General overview of rootReducer
The root reducer can get big if there is a lot of object and it is not convenient to have to go through all the reducers to have an overview of the state structure for each of its key. Writing  a full reducer with all its states in the rootReducer file as a `comment` could be a good idea. 


#### Parent / Children

It become a little hard sometimes to follow and all the props and function passed down to children!


#### State in sync with data in server

One of my main concern here was wheter I should use the state data or call APIs whenever I loaded a component. On one hand, using only the state is fast but there is a risk of data updating on the server while, hence have an state that is not up to date. Indeed, API was made to be integrated with other app, so it is not excluded that many app are connected to the same API.  I guess we should find the right balance between both.


#### Route withRouter

For some of my nested component, I needed the to be able to get `match` or use `history`, but some of them were two levels deep. It is quite anmoying to pass down props 2 levels deep, It is hard to follow the flow, therefore hard to debug. `react-router-dom` comes with a great method call `withRouter`, which will allow you to have access to `match`, `history` and `location`. Just wrap the component with `withRouter` and it will be available as a `props`.

`export default withRouter(MyComponent)` or `export default withRouter(connect(null,null)(MyComponent))`


## Conclusion

Creating this app was quite challenging, moreover the courses on React and Redux are much more concise than the one on Ruby and JS. But it was quite fun to build having to deal with RAILS API and React Redux for the frontend! Took me definitevely more time than expected, but I learnt a lot.
