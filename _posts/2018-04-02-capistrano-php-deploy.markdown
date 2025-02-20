---
layout: post
title:  "Automate PHP deploy with Capistrano"
date:   2018-04-02 20:30:00 +0200
categories: coding javascript jquery select dropdown
excerpt: "Using Capistrano, arbitrary functions and procedures can be performed on virtual servers without direct interference by having Capistrano execute a script with all the instructions listed."
---

Capistrano is a Ruby programming language based, open-source server (or deployment) management tool. Using Capistrano, arbitrary functions and procedures can be performed on virtual servers without direct interference by having Capistrano execute a script (i.e. a recipe) with all the instructions listed. In a general sense, this tool can be considered a developer’s very own [deployment assistant][capistrano-tips]{:target="_blank" rel="noopener"}, helping with almost anything from [getting the code on the remote machine][capistrano-php]{:target="_blank" rel="noopener"} to bootstrapping the entire getting-online process.

<h3>Install</h3>

{% highlight bash %}
gem install capistrano --no-ri --no-rdoc
{% endhighlight %}

<h3>Initiate</h3>

{% highlight bash %}
cap install

# mkdir -p config/deploy
# create config/deploy.rb
# create config/deploy/staging.rb
# create config/deploy/production.rb
# mkdir -p lib/capistrano/tasks
# Capified
{% endhighlight %}

<h3>Config deploy</h3>

> config/deploy.rb

{% highlight bash %}
# Define the name of the application
set :application, 'my_app'

# Define where can Capistrano access the source repository
# set :repo_url, 'https://github.com/[user name]/[application name].git'
set :scm, :git
set :repo_url, 'https://github.com/user123/my_app.git'

# Define where to put your application code
set :deploy_to, "/var/www/my_app"

set :pty, true

set :format, :pretty

# Set your post-deployment settings.
# For example, you can restart your Nginx process
# similar to the below example.
# To learn more about how to work with Capistrano tasks
# check out the official Capistrano documentation at:
# http://capistranorb.com/

# namespace :deploy do
#   desc 'Restart application'
#   task :restart do
#     on roles(:app), in: :sequence, wait: 5 do
#       # Your restart mechanism here, for example:
#       sudo "service nginx restart"
#     end
#   end
# end
{% endhighlight %}

<h3>Config prod</h3>

> config/deploy/production.rb

{% highlight bash %}
# Define roles, user and IP address of deployment server
# role :name, %{[user]@[IP adde.]}
role :app, %w{deployer@162.243.74.190}

# Define server(s)
# Example:
# server '[your droplet's IP addr]', user: '[the deployer user]', roles: %w{[role names as defined above]}
# server '162.243.74.190', user: 'deployer', roles: %w{app}
server '162.243.74.190', user: 'deployer', roles: %w{app}

# SSH Options
# See the example commented out section in the file
# for more options.
set :ssh_options, {
    forward_agent: false,
    auth_methods: %w(password),
    password: 'user_deployers_password',
    user: 'deployer',
}
{% endhighlight %}

<h3>Deploy</h3>

{% highlight bash %}
cap production deploy
{% endhighlight %}

[capistrano-php]: https://www.digitalocean.com/community/tutorials/how-to-automate-php-app-deployment-process-using-capistrano-on-ubuntu-13
[capistrano-tips]: http://guides.beanstalkapp.com/deployments/deploy-with-capistrano.html