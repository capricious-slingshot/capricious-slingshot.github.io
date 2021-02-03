---
layout: post
title:      "Glowing Spork Recipes"
date:       2021-02-03 14:54:58 -0500
permalink:  glowing_spork_recipes
---


**Tldr;** - All in all My project a very basic app - anyone can search and view public recipes or create an account via the app itself or Github Omniauth. Any authenticated user can create, save or rate recipes, as well as edit their personal profile.



Rails is a fairly bloated, overly engineered, and overkill for most any small personal project you would want to undertake. I understand the thought that it is easy to get ‘up and running’ with a web app but the fact of the matter is that there are a number of bad practices that are aimed at accomplishing this goal that they create so much overwhelming garbage (*I’m looking at you rails generate*) that you just want to light a match and go learn an entirely different language. 

Rails relies on convention over configuration, and while this does a lot of the heavy lifting, it also overly abstracts things to the point that you feel like you’re just spitting out harry potter incantations and not really understanding the what or the why of your actions. I am extremely uncomfortable with that. Abstracting things so far off from basic web concepts that you start to forget core principals that should be common knowledge (forms, abstracting rake, forms, class macros - readers/writers etc, did I mention the 87 ways to do forms? ). If you’ve done things a thousand times and really understand the underlying principals I could see the attraction, but for so many entry level developers it’s like unleashing them on the world with a broken wand.  I can relate to the concept of “omakase” that the authors are trying to ‘serve up’, but the wild goose chase to understand some of the architectural decisions is infuriating. There's a disconnect with that metaphor, however, becasue developers are *also* responsible for creation and not purely consumers. Rails does a fair job at handling security holes and staying abreast and up to date with those, but that’s the whole point of using a framework isn’t it? I like Ruby, I really enjoy the simplicity of Sinatra and honestly wish there was more in this realm, but as for my opinion of Rails…the jury is out for now.

For my project I went with the generic suggestion of building a recipe card application. I wasn’t particularly jazzed about the topic, but it was an averaged sized problem that presented several different ways you could approach it. Unoriginality drives me insane, but the ability to architect my own approach to solving the problem was very appealing.

The first step was to bang out my schema - I don’t really care for any of the fancy DB Schema software, I find it much easier to just bang it out in my text editor. Nothing a few cups of coffee can’t solve. I didn’t have too many issues or deviations from the original schema outside of idiot human errors. Turns out spelling is hard - I repeatedly misspelled ‘Recipe’ through the project and lost epochs of time fixing all of the things.

The next step was to stand up the app and get something I to look at - and after doing a little research I decided to go with Bulma. I’ve worked with Bootstrap and Foundation in the past and wanted to try something new. Bulma is lightweight and seemed like a pleasant alternative to the bloated backend. I banged out a simple Recipe index and show page with corresponding model to get something to look at.

Authentication followed, created a separate layout for that piece and stood it up but failed miserably on Github Omniauth. Turns out not only do you have to have your profile set to public, but you also have to select the email that you wish to interact through even if you only have one associated with your account. This was a real head scratcher - the public vs private bit was obvious, but the second step was not well documented at all. I punted on the omniauth until the very end, which introduced it’s bugs into the simple auth that I had created.

Next step was to blow away minitest and set up with Rspec. There isn’t any support in the curriculum for testing or deployment, so I got the basic framework up an hacked a few tests in. The original thought was that I would skip over testing for the purpose of brevity and time, but then as my application became more complex, I really hated myself for not taking the time to properly build them out. This is something that I need to finish, I’m not comfortable leaving these two areas unfinished.
 

Nested forms was the bane of my existence for longer than it really should have been, but if something is worth doing, it’s worth doing properly and I rejected any suggestion to dumb things down. It was the only semi-interesting piece in this project. I went with a double nesting and then used the [Cocoon gem](https://github.com/nathanvda/cocoon/) on top to dynamically handle the addition/subtraction of field elements by the user. It eventually came together, and in hindsight it actually wasn’t that difficult. You just need to be able to get feedback form someone that knows what they’re doing. (*Thanks Dustin & nathanvda!*). 


There are a few other JS touches that I would really like to come back and add in when I learn how. Minor things like dismissing notices, fixing the mobile navigation, as well as the display of user recipe tabs. Similarly the search only goes so deep - it was mentioned that JS is the faster way to handle this for the sake of the load on the server and rendering time. I still would have liked to be able to handle this on the server side, purely for the sake of understanding how things work and how they compare and contrast.

*Some minor things of note, for the sake of consistency:*

*In some of the partials I chose to incorporate conditional logic in different ways for the sake of demonstration. In the Users form partial I chose to use a `<% @user.persisted? %>` conditional within the template itself to hide things like a delete button, and other form fields that would only be needed when a user was updating their profile. An alternative way fo doing this would to use a contentfor block with the extra HTML included in the corresponding action view and then called later in the template itself, as I did within the form partial for UserRecipeCard. Honestly, it’s 6 of one, half dozen of the other. Personally I prefer keeping all of the HTML in one place so that you don’t have to jump files like crazy. Yes, conditional logic technically belongs in a helper method, and views should be kept dumb, but parsing and stitching together a million files that form a single view ultimately is a nightmare. It’s just personal preference. I did prefer the `contentfor` when passing variables such as the name of the title bar that appears in Application Controller.*

