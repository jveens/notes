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
So it will be _'root pages#home'_

## 6. Create more pages

Other pages can be created by adding additional actions to our pages controller.
For example, we can create an about page by adding an 'about' action. 

	def about
		# action code goes here
	end

Then, add the appropriate view: _app/views/pages/about.html.erb_

And add our route: _get "about" => "pages#about"
