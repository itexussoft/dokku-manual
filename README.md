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
(These command are actual for 2016-09-15.)

## Preparations with Dokku
When Dokku is installed you need to setup it. Enter your instance *public IP* in the browser - and you will go onto Dokku admin setup.

![Dokku Admin Setup][https://raw.githubusercontent.com/nastia-shaternik/dokku-manual/master/images/dokku-admin-setup.png]

## Preparations with rails app

## Deployment
