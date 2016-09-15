# Dokku Manual

In this manual we gonna deploy sample rails app (backed w/ PostgreSQL)
on [Dokku](https://github.com/dokku/dokku)
and describe each action step by step.

**FYI**: [App](https://github.com/nastia-shaternik/dev) that we use in this deployment session.

## Dokku Instalation
We will run Dokku on [AWS EC2](https://aws.amazon.com/ec2) instance. How to setup an instance is not a part of this guide. Make sure you have ability to ssh on your instance.
Also have a look at [Digital Ocean](https://www.digitalocean.com/products/one-click-apps/dokku/) where you can choose an instace with already installed Dokku preset.

So ssh on your EC2 instance and install Dokku with 2 commands:

```bash
wget https://raw.githubusercontent.com/dokku/dokku/v0.7.1/bootstrap.sh
sudo DOKKU_TAG=v0.7.1 bash bootstrap.sh
```
(These commands are actual for 2016-09-15.)

## Preparations with Dokku

### Dokku Admin Setup
When Dokku is installed you need to setup it. Enter your instance *public IP* in the browser - and you will go onto Dokku admin setup.

![Dokku Image Setup](https://raw.githubusercontent.com/nastia-shaternik/dokku-manual/master/images/dokku-admin-setup.png)

Ensure you've pasted your *public key* (served on machine you'll
deploy from) or you'll lost an ability to deploy at all.

As HOSTNAME leave your instance public IP for now, every deployed app will be
available though port (don't forget to open it later).

### Create Dokku app and link DB
Now we need do some preparations for new app. On your Dokku host run
these commands:

```bash
dokku apps:create #{APP_NAME}

# install postgress plugin if needed
sudo dokku plugin:install https://github.com/dokku/dokku-postgres.git

# create a postgres service with the name
dokku postgres:create #{DB_NAME}

# link app with DB store
# each official datastore offers a `link` method to link a service to any application
dokku postgres:link #{DB_NAME} #{APP_NAME}
```

The last command will create ENV variable *DATABASE_URL* that you can
use in `database.yml` to setup production DB:

```yml
production:
  adapter: postgresql
  encoding: unicode
  pool: 5
  url: <%= ENV['DATABASE_URL'] %>
```

## Preparations with rails app

Consider to add `Procfile` - [where you can define](https://devcenter.heroku.com/articles/procfile) processes that are going to be used.

Sample `Procfile`:

```
web: bin/rails server -p 5000 -e production
```
Consider to move POST and RAILS_ENV into the ENV vars.

Also, if needed, add `gem 'rails_12factor', group: :production` into your `Gemfile`. Read about it [here](https://github.com/heroku/rails_12factor).

## Deployment

Dokku suports deployment using Dokerfile or Docker image or tarfile or buildpacks. The last method
that we're using. To deal with buildpacks Dokku uses [Herokuish](https://github.com/gliderlabs/herokuish).

Firstly, we need to add remote:

`git remote add dokku dokku@#{PUBLIC_IP}:#{APP_NAME}`

`git push dokku master`

![Dokku Deploy Start](https://raw.githubusercontent.com/nastia-shaternik/dokku-manual/master/images/dokku-admin-setup.png)

![Dokku Deploy End](https://raw.githubusercontent.com/nastia-shaternik/dokku-manual/master/images/dokku-admin-setup.png)

## Conclusion
