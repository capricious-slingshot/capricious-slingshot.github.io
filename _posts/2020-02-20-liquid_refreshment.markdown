---
layout: post
title:      "Liquid Refreshment"
date:       2020-02-20 16:29:05 -0500
permalink:  liquid_refreshment
---


For my Sinatra project I decided to create an application for a local brewery that I frequent. The breweries name is Cloudburst, and they release a handful of unique beers every month to a rotating roster of beers both,  light and dark, most of which are never to be seen or heardfrom again. Every year, they have a big birthday bash and invite people to vote for their favorite beer from the past year. The problem is that they have such random names with crazy nonsensical descriptions that it's virtually impossible to look at a list and remember what you tried, let alone what you liked in order to bring one back for their party. Liquid Refreshment is a sinatra application that I created to solve that problem.

Liquid Refreshment is CRUD in two different parts - first there is a wikipedia style entry at the beer level. Any registered user can create or update a beer (some of the desriptions can be crazy long and nonsensical). A Beer exzists entirely on it's own, but a User can also make a personal Opinion about a beer such as a rating of 1 - 5 stars and also add in any individual tasting notes that they can refer back to later. Personal Opinions are only visible and updateable to the user that created them. One thing that I wanted to implement in this project is a fuzzy search - I have a lookup upon Beer creation for a matching title and description, but it's looking for an exzact match and therefor not as effective. 

This project also tackeled the problem of user authentication. I used bcrypt as my engription gem, and users can create/delete accounts as well as update thier passwords and user names. Another item that I would have liked to implement here is a mailer, for authentication purposes, and also to get this up on heroku, but I wasn't able to get it to work on my own out of the box initally, and that isn't something that is covered in the cirriculum. I have mixed feelings about pushing it up now without the missing pieces I covered above as I feel like not having those doen't really make this a production worthy app. I also have some questions about general security issues that I may be completely blindsighted to as a new developer, but I guess I will have to save those for another day.
