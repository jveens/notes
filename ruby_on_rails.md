# Getting started with Ruby on Rails

## 1. Install Rails
Follow directions [here](http://installrails.com/).

##2. Create a new application

The following commands create a new app based on a sqlite database.

	$ rails new < appname > 
	$ cd < appname >

If we want to deploy to heroku, we will need to switch our production database to postgres or create with with postgres from the beginning: 

	$ rails new < appname > --database=postgresql

##3. Run our rails server

	$ rails server
	OR
	$ rails s

