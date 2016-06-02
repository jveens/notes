# Getting started with Ruby on Rails

## 1. Install Rails
Follow directions [here](http://installrails.com/).

## 2. Create a new application

The following commands create a new app based on a sqlite database.

	$ rails new < appname > 
	$ cd < appname >

If we want to deploy to heroku, we will need to switch our production database to postgres or create with with postgres from the beginning: 

	$ rails new < appname > --database=postgresql
	
Install PostgresApp to easily run a local PostGres server: http://postgresapp.com/documentation/

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

	$ heroku run rake db:migrate

	$ rake db:migrate:status // check the status of your migrations

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

## 14. Add a Scaffold to our Application

A model represents whatever bits of data our application uses. Think of a Tweet in twitter, or a status update in Facebook. 

We can manually create a Model and all the needed controller actions and paths, but Rails gives us a bit of 'Magic' to do this quickly:

	$ rails generate scaffold < model_name_plural > < db_column:variable_type > 

When we do this Rails generates a stylesheet for the scaffold, but we probably don't need it so feel free to delete it: _app/assets/stylesheets/scaffolds.css.scss_ Or just skip installing it altogether: 

	$ rails generate scaffold < model_name_plural > < db_column:variable_type > --skip-stylesheets

## 15. Associations

At some point, we're going to want to associate the bits of our app together. For example, a user owns tweets and tweets belong to a user. 

To do this, we need to add a column to our resource to store the user_id. 

	class Tweet < ActiveRecord::Base
		belongs_to :user
	end

	$ rails generate migration add_user_id_to_tweets user_id:integer:index

	class User < ActiveRecord::Base
	  # Include default devise modules. Others available are:
	  # :token_authenticatable, :confirmable,
	  # :lockable, :timeoutable and :omniauthable
	  devise :database_authenticatable, :registerable,
	         :recoverable, :rememberable, :trackable, :validatable

	  has_many :tweets
	end

## 16. Rails Console

The rails console can allow us to interact with our Database. It can be useful to find out what's going on with our app, and if things are going wrong it can help with debugging. 

	$ rails console
	$ rails c // shortform

Let's check if our associations are working: 

	$ Tweet.connection #Connect to the DB
	$ Tweet.inspect #shows all of the parameters for a tweet 
	$ tweet = Tweet.first #make sure Tweet.first is UPPERCASE
	$ tweet.user


	$ tweet = Tweet.first
	$ tweet #Check out the tweet!
	$ tweet.user_id = 1
	$ tweet.save

	$ user = User.first 
	$ user.tweets

	#Now we can call these methods
	$ user.tweets
	$ tweet.user

## 17. Authorizations
We probably want to add some authorizations to our apps. We want to lock down some parts for people who are logged in, and we want to allow people to use other parts of our app, even if they are not logged in (the homepage or sign up pages for example.)

Add a before_action to lock down only specific pages:

	before_action :authenticate_user!, except: [:index, :show]
	before_action :correct_user, only: [:edit, :update, :destroy]

Add a new private method to our cotroller that checks if it's the correct user:

	def set_user
      @resource = current_user.resources.find_by(id: params[:id])
      redirect_to resources_path, notice: "Not authorized to edit this resource" if @resource.nil?
    end

In the resource view:
	
	<%= resource.user.email if resource.user %>
	OR
	<%= resource.user.try(:email) %>

In our header partial, we'll want to conditionally show and hide links (ex. only show login link if not logged in, show logout link if already logged in).

In our resource.html.erb we want to conditionally show and hide edit and delete links (only show these links if it's the user who owns them).

	<% if resource.user == current_user %>
	  <%= link_to 'Edit', edit_resource_path(resource) %>
	  <%= link_to 'Destroy', resource, method: :delete, data: { confirm: 'Are you sure?' } %>
	<% end %>

## 18. Handling file uploads
Maybe we want our users to be able to upload images or files?
We'll use a popular gem called Paperclip for this https://github.com/thoughtbot/paperclip. A prereq is ImageMagick, so we'll need to install that too. http://cactuslab.com/imagemagick/

To check if it's installed: 
	
	$ identify

In our gem file, add paperclip:

	gem 'paperclip', '~> 4.2'
	OR
	gem "paperclip", "~> 5.0.0.beta1"

	$ bundle install
	// restart server

	$ rails generate paperclip resource image

Edit your resource form: 

	<%= form_for @resource, html: { multipart: true } do |f| %>
	.
	.
	.
	  <div class="form-group">
	    <%= f.label :image %>
	    <%= f.file_field :image, class: "form-control" %>
	  </div>
	.
	.
	.

Update resource parameters to allow image: 

	def resource_params
      params.require(:resource).permit(:description, :image)
    end

To include an image in erb: 

	<%= image_tag @resource.image.url(:medium) %>
	// check out paperclips docs for more info on sizes

We'll also need to set up Amazone Web services to handle our uploaded images, since we can't store them on Heroku.

## 19. Adding Pagination

Our resources might get to be too much to serve on one page. There's a pagination gem that can help us here:

https://github.com/mislav/will_paginate#basic-will_paginate-use

	gem 'will_paginate'
	OR 
	gem 'will_paginate-bootstrap'

In our resources controller: 

	def index
	  @resources = Resource.all.order("created_at DESC").paginate(:page => params[:page], :per_page => 8)
	end

Render the pagination in our views: 

	<%= will_paginate @resources %>
	OR
	<%= will_paginate @resources, renderer: BootstrapPagination::Rails %>

20. Adding extra info to Users
At some point, it may be a bit of a security risk to always show our user emails. We can add their name or a username to show instead. 

	$ rails generate migration add_name_to_users name:string

	$ rake db:migrate
	AND 
	$ heroku run rake db:migrate

Edit the devise form so that Users have to enter a name when registering and editing their profile:

	<div class="form-group">
       <%= f.label :name %>
       <%= f.text_field :name, class: "form-control", :autofocus => true %>
     </div>

In our application controller: 

	class ApplicationController < ActionController::Base
	 # Prevent CSRF attacks by raising an exception.
	 # For APIs, you may want to use :null_session instead.
	 protect_from_forgery with: :exception
	 before_filter :configure_permitted_parameters, if: :devise_controller?

	protected

	 def configure_permitted_parameters
	   devise_parameter_sanitizer.for(:sign_up) << :name
	   devise_parameter_sanitizer.for(:account_update) << :name
	 end
	end

In our views, where we were referenceing the user.email:

	<%= resource.user.name if resource.user %>

## 21. Validations

Sometime we want to validate the data our users are putting in, both to make sure it exists, and to make sure it is the proper type or format. http://edgeguides.rubyonrails.org/active_record_validations.html

In our model:

	validates :name, presence: true














