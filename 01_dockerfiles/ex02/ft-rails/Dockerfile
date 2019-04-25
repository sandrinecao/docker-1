#You are going to create your first Dockerfile to containerize Rails applications. That's
#a special configuration: this particular Dockerfile will be generic, and called in another
#Dockerfile, that will look like this:

#FROM     ft-rails:on-build
#
##EXPOSE      3000
#CMD      ["rails", "s", "-b", "0.0.0.0", "-p", "3000"]

#Your generic container should install, via a ruby container, all the necessary dependecies
#and gems, then copy your rails application in the /opt/app folder of your container. Docker
#has to install the appropriate gems when it builds, but also launch the migrations and the db
#population for your application. The child Dockerfile should launch the rails server. If you
#don't know what commands to use, it's high time to look at the Ruby on Rails documentation.

FROM ruby:2.5.1

RUN apt-get update && apt-get -y upgrade && \
    apt-get install -y nodejs

ONBUILD COPY app /opt/app
ONBUILD WORKDIR /opt/app

ONBUILD EXPOSE 3000
ONBUILD RUN bundle install
ONBUILD RUN rake db:migrate
ONBUILD RUN rake db:seed

# docker build -t ft-rails:on-build .
# test app: https://github.com/sandrinecao/rails-docker-1-app.git
