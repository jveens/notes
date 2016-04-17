# Getting started with Ruby on Rails

## 1. Install Rails
Follow directions [here](http://installrails.com/).

## 2. Create a new application

The following commands create a new app based on a sqlite database.

	$ rails new < appname > 
	$ cd < appname >

If we want to deploy to heroku, we will need to switch our production database to postgres or create with with postgres from the beginning: 

	$ rails new < appname > --database=postgresql

## 3. Run our rails server

	$ rails server
	OR
	$ rails s

## 4. Create our home page

For a new page to start, we'll generate a pages controller with a home action. 

	$ rails generate controller pages home

_Controllers always reference the plural form of the word._

We can now change the way our page looks by accessing the view: 

	app/views/pages/home.html.erb

## 5. Route our app so our homepage shows

Routes are controlled in the file: _config/routes.rb_

We can set the root of our app by: _"root controller#action"_
So it will be _"root pages#home"_

## 6. Create more pages

Other pages can be created by adding additional actions to our pages controller.
For example, we can create an about page by adding an 'about' action. 

	def about
		# action code goes here
	end

Then, add the appropriate view: _app/views/pages/about.html.erb_

And add our route: _get "about" => "pages#about"_

## 7. Creating views - ERB (Embedded RuBy)

ERB files allow us to embed ruby code directly into our HTML. 
Ruby can be calculated with <% %> brackets and printed with <%= %> brackets. 

Ruby has lots of helpers that can output HTML. 

Create a link :
	<%= link_to "link text here", "link path here" %>

We can also use path helpers Ruby gives us: 
	<%= link_to "link text here", helper_path %>

## 8. Easily style our App with Bootstrap

[Bootstrap](https://getbootstrap.com/) is a front-end framework that can help us get a decent looking site up in almost no time!

There's a gem that makes Bootstrap easily integrate into our Rails projects: [https://github.com/twbs/bootstrap-sass]

Add the gem to our gemfile: _gem 'bootstrap-sass'_

Whenever we add a new gem to our application, we have to do bundle install and restart our rails server:

	$ bundle install
	$ rails s

## 9. Set up Bootstrap

All the Sass files for our project are found in the assets file. 
The applicaiton.scss is all the styles for our application. 
View specific files can be plaes in view.scss.

In our application.scss import the necessary bootstrap files: 
	@import "bootstrap-sprockets";
	@import "bootstrap";

Remove all the *= require_self and *= require_tree . statements from the sass file. Instead, use @import to import Sass files.

We also have to add the Bootstrap JS for things to work properly. In our JavaScript assets:
	//= require jquery
	//= require bootstrap-sprockets

Note: (from https://github.com/twbs/bootstrap-sass) bootstrap-sprockets and bootstrap should not both be included in application.js.
bootstrap-sprockets provides individual Bootstrap Javascript files (alert.js or dropdown.js, for example), while bootstrap provides a concatenated file containing all Bootstrap Javascripts.

## 10. ERB Partials
To better reuse code, it's sometimes a good idea to create partials for bits of our application that we want to reuse (ex. Header and Footer).
To create a partial, name the file with a leading underscore (_header.html.erb)
Move any code you want to be a part of the Header into the _header file. Then in yoru view, reference the partial: 

	<%= render 'layouts/header' %>

It can be a good idea to add a viewport meta tag to your application, to ensure it looks as it should on all devices. 
	<meta name="viewport" content="width=device-width, initial-scale=1.0">

## 11. Deploying with Heroku
Ensure that you have an account with Heroku, and have downloaded the Toolbelt: https://toolbelt.heroku.com/

Set up your terminal for deployments:
	
	$ heroku login
	$ heroku keys:add
	$ heroku create

If we're not already using Postgres, we need to set up separate dev and production environments. 
To deploy our app: 
	
	$ git push heroku master

To open our app, or to change our url:
	
	$ heroku open
	$ heroku rename < new appname >

Debugging Heroku:
Sometimes we'll get an error message from Heroku. To find out what went wrong: 
	
	$heroku logs --tail

## 12. Adding User accounts with Devise
	
In our gemfile: gem 'devise'

Once you bundle, we can install devise: 
	
	$ rails generate devise:install

Once you install, Devise will give you a set of instructions. Ensure you've completed these steps.

Set precompile to false _config/application.rb_: config.assets.initialize_on_precompile = false

Installing Devise views is optional, but we can do this if we'd like to customize the way our login and sign up pages look: 

	$ rails g devise:views

Now we're ready to add our user models with Devise:

	$ rails generate devise user

Anytime we alter our database, we need to perform a migration to make the changes:

	$ rake db:migrate
	// restart your server

## 13. Managing our routes
All our routes can be found by doing: 

	$ rake routes

This gives us all the routes and their actions, as well as shows us the helper paths we can use. 
We want to be sure that our users can sign in or log up: 

	_app/views/layout/_header.html.erb_

	<ul class="nav navbar-nav navbar-right">
        <li><%= link_to "Home", root_path %></li>
        <li><%= link_to "About", about_path %></li>
        <% if user_signed_in? %>
          <li><%= link_to "Log out", destroy_user_session_path, method: :delete %></li>
        <% else %>
          <li><%= link_to "Sign in", new_user_session_path %></li>
        <% end %>
    </ul>










