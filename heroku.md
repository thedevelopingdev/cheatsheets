# Heroku cheatsheet

<!-- vim-markdown-toc GFM -->

* [Client configuration](#client-configuration)
        * [Configure the default app](#configure-the-default-app)
* [Deployment](#deployment)
        * [Set the environment variables in Heroku from a `.env` file](#set-the-environment-variables-in-heroku-from-a-env-file)

<!-- vim-markdown-toc -->

## Client configuration

#### Configure the default app

Use the environment variable `HEROKU_APP`.

## Deployment

#### Set the environment variables in Heroku from a `.env` file

Format the `.env` file so that there are no spaces around the "equals" sign.

```sh
$ cat .env
# FIRST_VAR_NAME=value1
# SECOND_VAR_NAME=value2

$ heroku config:set $(cat .env)
```
