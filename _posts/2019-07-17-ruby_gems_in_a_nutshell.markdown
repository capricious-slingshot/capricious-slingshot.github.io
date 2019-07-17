---
layout: post
title:      "Ruby Gems In A Nutshell"
date:       2019-07-17 19:06:05 -0400
permalink:  ruby_gems_in_a_nutshell
---


A ruby gem is a packaged Ruby library or program that is created by an individual to solve a specific problem or add specific functionality. Gems are used in Ruby to avoid duplication and prevent reinventing the wheel.

Each gem has 3 key parts: **name**, **version**, and **platform**.

The **gem name** is completely arbitrary, but generally has some correlation to the problem it is trying to solve. Every published public gem *must* have a unique name. Private or Local gems must also have a unique name within their namespace, but require an additional ``:path`` option to avoid a Bundler mishap with any public gems.

The gem version, much like Ruby’s own version, follows [semantic versioning](https://semver.org). This consists of 3 numbers, separated by periods, reading left to right in order of importance. Version numbers are similar to a car milage counter; when a number on the left changes, everything to the left is zeroed out and the count begins again. A **pre-release** is any version beginning with zero, and is a way of signaling to users ‘this is under rapid development, use with caution’.

***MAJOR** – Leftmost position. Increments only when backwards incompatible API changes occur. Updating after such a change will most likely break your codebase and should be fully tested before deployment.

***MINOR** – Center position. Increments any time new functionality/features are added. All changes in this category are backwards compatible to the last major release.

***PATCH** – Rightmost position. Increments for implementation level detail changes and bug fixes.

**gem platforms** are based on CPU architecture and OS type. The platform simply indicates that the gem will only work on a matching OS type (OSX, Windows, Ubuntu, etc). RubyGems automatically checks your platform and downloads the correct version for you.

### Installing a gem

There are two ways to install a gem on your computer.

1.   Locally from the Command Line

A program called bundler will fetch the latest gem version for your platform from Rubygems.org, or in the case of vendor specific gems, from the specified ``:path``.

      bundle install gem_name
			

      gem 'extracted_library', '~> 1.2', :path => './vendor/extracted_library'

These gems are installed generally around the user root of your computer, depending on your OS and Ruby Version. This location can be viewed using:

      gem list                              (all local gems)

Bundler will not only install the gem that you requested, but it will also install any dependencies needed for it to run. This dependency list is packaged within each gem itself and is automagically handled for you by bundler. 

2.   At the Application Level via Gemfile

Bundler will fetch the latest gem version for your platform from the source set at the top of your gemfile, or in the case of vendor specific gems, from the specified ``:path``.
			 
This location can be viewed using:
			 
			gem list sinatra -d                   (author/details of specific gem)
			 
More on the specifics of gemfiles below.


To quote [bundler.io](http://https://bundler.io):

### *“Bundler is an exit from dependency hell”*

For the purposes of this post, just know that there is way more going on with bundler than can be seen by the naked eye. It makes developer life easy by ensuring that the gems you need are present and compatible at varying levels of development, testing, and deployment.

When writing programs and developing applications, the preferred method of installing gems on your machine is to use a Gemfile. Running ``bundle install`` from the root of your program will install all required gems. ``require 'gem_name' `` at the top of your file will then make it accessible to Ruby, by putting it on the load path. This is a cleaner way of keeping track of what gems are running in each individual program, prevents buggy behavior by forcing your program to use a specific gem version, and most importantly allows anyone pulling down your code to get up and running with the correct gem set quickly.
			 

			 
#### Gemfiles

A Gemfile is simply a plain text file that holds all of the gem dependencies needed for your application to run Ruby. Gemfiles require at least one gem source, in the form of the URL for a Rubygems server. Upon creation of your project, running

      bundle init

will generate a new Gemfile at the root of your project directory and add the default ‘https’ protocol source for [rubygems.org](http://rubygems.org) automatically. This verifies your connection to the rubygems.org server will be verified with SSL. You can then add/remove gems from this file over time as needed.

All gems should be declared, including version numbers. Without going into too much detail about ‘optimistic’ vs. ‘pessimistic’ version constraint and the myriad of specifiers that come with them, just know that omitting this number following your gem name will cause Bad Things to happen. And thus in turn, may cause you to Bang Your Head and lose sleep. Don’t lose sleep. Sleep is delightful.

RubyGems provides a ‘happy medium’ shortcut for version constraint called the [twiddle-wakka](https://thoughtbot.com/blog/rubys-pessimistic-operator). This convention ignores the **PATCH** level part of the version number (allowing it to increase), and will allow any **MINOR** changes (standards dictate that these must be backwards compatible) up to but not including the next **MAJOR** release.

      gem 'library', '~> 2.2'

      gem 'extracted_library', '~> 1.2', :path => './vendor/extracted_library'

Take away: guard yourself from potential bugs/failures/sleeploss by using the twiddle-wakka shortcut instead of not declaring a version at all. For Alfred 2 powerpack users using rubygems as your source, I highly recommend [this extension](http://https://github.com/BlueVajra/ruby_gem_workflow) for ease of daily life (only alfred 2).

### Gemfile Groups

For programs or applications that require a large Gemfile (such as Sinatra or Rails) your gem declarations can be further organized into groups to limit what gets loaded in particular environments. The most commonly used groups are: development, testing, staging, and production. They can be declared one of two ways:

A `` do   end `` block for multiple gems within a group:

      group :test do
           gem 'rspec', '~> 3.0.0'
           gem 'capybara', '~> 2.4.1'
      end

or in the same line as your declaration for smaller subsets or standalone gems:

      gem 'rspec', '~> 3.0.0', :group => :test

      gem 'cucumber', '~> 1.3.15', :group => [:cucumber, :test]

Any gems that are not included inside groups are considered default, and will be automatically loaded everywhere. Grouping allows you to pass flags when running ``bundle install`` to omit unnecessary gems (see: databases) from being loaded onto your computer:

      bundle install --without staging production

Additionally, many frameworks (e.g. Rails) will require a specific group all at once by default:

      Bundler.require(:development)

### Gemfile.lock

This is the mysterious file that is automagically generated when you run bundle install at the application level. This file is only read by the computer (ie: don’t touch). It looks virtually identical to your Gemfile, but is actually a resolved set of dependencies, and it doesn’t change over time. Think of this file as a working ‘snapshot’ of the Gemfile.

Bundler loads the gem declarations, their dependencies, and identifies any constraints between gems. It then selects the best available gem version that satisfies all constraints within a gemfile and locks it down. The `` .lock `` will always be used unless explicitly updated, even if a bigger/better/faster version is released that satisfies these constraints. ``Gemfile.lock``  can also be a safety precaution when updating gems, but that is another topic entirely that I will save for another day.

###### ***RESOURCES:***
###### [bundler](https://bundler.io/v1.6/bundle_install.html)
###### [rubygems.org](https://rubygems.org)
###### [semantic versioning](https://semver.org)
###### [patterns](https://guides.rubygems.org/patterns/)
###### [what-is-a-gem](https://guides.rubygems.org/what-is-a-gem)
###### [modern-day-ruby-warrior](http://rubylearning.com/blog/2010/10/06/gem-sawyer-modern-day-ruby-warrior/)
