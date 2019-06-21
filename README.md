This is the exmaple of modern Ruby on Rails application. 
It consists of RoR 6, Postgres, Redis, Webpack, React. 
All services are described in the docker-compose.yml.

# How to run?

This application customized that we can run it in two ways. Either run by Docker Compose only or by DIP. 
This way we can smoothly and gradually move members of your team to use DIP. 
For veterans is nothing change. They as before run the application by `docker-compose up rails webpack` 
and open `localhost:3000` in a browser. 
BTW, you can try it now after hand setup (see provision section in dip.yml). 
For progressive users (yeap, I mean us) everything is simple. 
I hope you already read [how to set up local dns](https://github.com/bibendi/dip/tree/master/docs) for getting the best experience. 
And Nginx must be running by `dip nginx up`. That's all we have to do once. After reboot, it will start automatically.
Also, I strongly recommend you [integrate DIP to your ZSH shell](https://github.com/bibendi/dip#integration-with-shell) 
for the best experience. 
Next simply run `provision` and the application will set up.
If you choose not the `.docker` top level domain then you must add `DOCKER_TLD=yourdomain` to `~/.bashrc` and reopen the console.
After that run `webpack` and in another console `rails s`. 
Also, you can leave webpack running in the background by `up -d webpack`. Where `up` is a alias for `docker-compose up`. 
Open http://dip-rails.docker and all should work.

# Usage

Assume that you have integrated DIP into ZSH shell, 
so all commands below run in a console without the `dip` "prefix".

```sh
# provision application
provision

# run rails app with all debuging capabilities (i.e. `binding.pry`)
rails s

# run rails console
rails c

# run webpack
up -d webpacker
# `-d` - mean that service will run in detached (background) mode
or
webpack

# run migrations
rake db:migrate

# pass env variables into application
VERSION=20100905201547 rake db:migrate:down

# run sidekiq
up -d sidekiq
or
sidekiq

# launch bash within app directory
bash

# Additional commands

# update gems or packages
bundle install
yarn install

# run psql console
psql app
# where `app` is a database name. It might be `app_test`.

# run redis-cli
redis-cli

# run tests
# TIP: `rspec` command is already auto prefixed with `RAILS_ENV=test`
rspec spec/path/to/single/test.rb:23

# run system tests
# webpack should be running with test environment
RAILS_ENV=test webpack
rspec spec/system

# shutdown all containers
down
```

# How this application has been created from scratch?

The main goal of using Docker is not having all dependencies installed on the host machine. 
So the first question is how to create an application without having rails? 
First of all, in an empty directory, we should add `Dockerfile`, `docker-compose.yml` and `dip.yml`. 
Then run `dip bash` where we can temporarily install `rails` gem by running `gem install rails`. 
After that, we have to generate the skeleton of our application. 
Run `rails new -d postgresql --skip-turbolinks --skip-bundle --webpack=react --skip --skip-test --skip-system-test .` 
Type `exit` and we will return into the host machine. 
Next, fix files permissions by running `sudo chown -R $USER:$USER .`. 
The last step we will need to run `dip provision`. 
That's all! Farther we customize the application how do you like. 
I strongly recommend learning the source code of this application. 
Maybe you simply decide to pick up it entirely because most pitfalls are found.


Will be continued...
