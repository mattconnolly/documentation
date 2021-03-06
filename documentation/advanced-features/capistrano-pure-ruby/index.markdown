---
title: Capistrano in ruby script
layout: default
---
Instead of building a config folder and deploy, you may want to programmatically set everything in a single ruby script. This could be done as follows:
{% prism ruby %}
  require 'capistrano/all'

  stages = "production"
  set :application, 'my_app_name'
  set :repo_url, 'git@github.com:capistrano/capistrano.git'
  set :deploy_to, '/var/www/'
  set :stage, :production
  role :app, %w{}

  require 'capistrano/setup'
  require 'capistrano/deploy'
  Dir.glob('capistrano/tasks/*.cap').each { |r| import r }

  Capistrano::Application.invoke("production")
  Capistrano::Application.invoke("deploy")
{% endprism%}

Note that the require order is important as the stage needs to be set before you load setup and deploy.
