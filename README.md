# What's in this repo?

This repo contains files created for an introductory lecture on [Jekyll](https://jekyllrb.com). The lecture is to be held in a [Frontend Zagreb Meetup](https://www.meetup.com/en-AU/FrontendZG/).

# Project structure

 * `1-simple-site` - demonstration of manual website build process
 * `2-using-themes` - how to use Jekyl themes
 * `3-github-pages` - how to create a guthub page

# Lecture notes

## Using themes

In this demo we'll first create a new website based on the default Jekyll theme called **Minima**. Then we'll install another teme and do a theme switch. 

### Creating a new website

To create a new website we'll use the `jekyll new` command:

    jekyll new ./jekyll-loves-wp

This command will do the following:

* create a directory for the new website 
* install the Minima theme
* copy sample files to the website directory


### Installing a new theme

In this step we will install a theme called `jekyll-theme-basic-basic` (from [Ruby Gems](https://rubygems.org/gems/jekyll-theme-basically-basic)).

A theme is installed like so:

    gem install jekyll-theme-basically-basic

### Enabling the theme for our new website

In this step we'll allow the use of the new theme in our website. To do that we need to edit the `Gemfile` file inside dir of the new website.

We'll add the following line, right below the `gem "minima"` line:

    gem 'jekyll-theme-cayman', '~> 0.1.0'

### Doing the theme switch

All we need to do now to switch our website to the new theme is edit the Jekyll config file `_config.yml`.

In that file we'll need locate the line defining the theme which is currently in use, comment it out and add the new theme below it:

    #theme: minima
    theme: jekyll-theme-basically-basic

To see the changes we need to re-build our website.


# Subjects

## Subjects covered by lecture
* _layout - how make the source DRY
* [_include](https://jekyllrb.com/docs/includes/) - how to insert content of other files
* _posts - how the posts work
    * **[paginator](https://jekyllrb.com/docs/pagination/)** - unavoidable subject when working with posts (a plugin which based on a given template automatically generates multiple pages, containing paginated posts)
* [liquid templates](https://jekyllrb.com/docs/templates/) - how to generate a menu & applying filters to transform data
* site.related_posts - showing related posts (last few)
* [sass](https://jekyllrb.com/docs/assets/) - using the `_sass` folder for partials
* [blog migration](https://jekyllrb.com/docs/migrations/) - just a mention that automatic migration is possible  
* [code highliting](https://jekyllrb.com/docs/templates/#code-snippet-highlighting) - useful for developers

## Subjects not covered in lecture
* [data files](https://jekyllrb.com/docs/datafiles/) - too specific ... this is tips & tricks kind of a subject
* [collections](https://jekyllrb.com/docs/collections/) - too advanced subject for an intorductory course
* [links to pages](https://jekyllrb.com/docs/templates/#link) - usefull, but we can get away without it - more of a tips & tricks kind of subject
* [include params & variables](https://jekyllrb.com/docs/includes/#passing-parameters-to-includes) - too advanced subject for an intorductory course

## Subjects to be researched
* **docs page**
  * [plugins](https://jekyllrb.com/docs/plugins/)
  * [themes](https://jekyllrb.com/docs/themes/)
  * [extras](https://jekyllrb.com/docs/extras/)
* **tutorials page**
  * [navigation](https://jekyllrb.com/tutorials/navigation/) - describes how to implement an effective navigation
