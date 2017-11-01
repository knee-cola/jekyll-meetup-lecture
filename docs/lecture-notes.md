---
layout: default
title: Lecture notes
---
# Using themes

In this demo we'll first create a new website based on the default Jekyll theme called **Minima**. Then we'll install another teme and do a theme switch. 

## Creating a new website

To create a new website we'll use the `jekyll new` command:

    jekyll new ./jekyll-loves-wp

This command will do the following:

* create a directory for the new website 
* install the Minima theme
* copy sample files to the website directory


## Installing a new theme

In this step we will install a theme called `jekyll-theme-basic-basic` (from [Ruby Gems](https://rubygems.org/gems/jekyll-theme-basically-basic)).

A theme is installed like so:

    gem install jekyll-theme-basically-basic

## Enabling the theme for our new website

In this step we'll allow the use of the new theme in our website. To do that we need to edit the `Gemfile` file inside dir of the new website.

We'll add the following line, right below the `gem "minima"` line:

    gem 'jekyll-theme-cayman', '~> 0.1.0'

## Doing the theme switch

All we need to do now to switch our website to the new theme is edit the Jekyll config file `_config.yml`.

In that file we'll need locate the line defining the theme which is currently in use, comment it out and add the new theme below it:

    #theme: minima
    theme: jekyll-theme-basically-basic

To see the changes we need to re-build our website.