---
layout: post-show
title: 
keywords--0: Gossip House - Planning
title--before: (Part I)
keywords--1: 
title--middle: 
keywords--2: 
title--after: 
---

'Gossip House' is going to be an app with a listing of posts, in chronological order, from all users are shown on a page. Names of the users who have made the posts remain anonymous if you are an outsider - that is, if you are not logged in yet.

It has taken a long time to get to this point:

* I have completed many exercises to do with MVC architecture.
* I have learned the basics of SQL commands - long queries terrify me still.
* I have completed 12 Chapters of Hartl and did a few chapters twice over.

Now I am tasked with rolling out this app. It will be super fun but the scale is huge. There needs to be a bit of planning for an overview of the app as it enables me to break the app down into sizeable chunks to be coded. On top of this, I need to be visualise the 'flow' or 'trend' moving from one point to another. Resources like 'Users' and 'Posts' will not come into existence until I have connected it between Routes, Controllers, Models, Views. Then I have to also consider concepts like Authentication and Authorization which will involve sessions, cookies, tokens and digests. Oh and I also need to test everything too. Dang.

So this blog series is not really going to be anything elaborate but it will convey my journey in accomplishing this task. Unfortunately it is going to represent the journey in a linear manner (read from top-bottom) instead of showing a mind-map which is probably closer to what is actually happening in my head and my planning.

So here goes:

`rails _5.0.0.1_ new gossip-house`

Now previously with Hartl's tutorials I `bundle install`ed a bootstrap gem to use that css framework. But I really do not like how bootstrap nests its boxes so deeply that specificity is thrown entirely out the window. This type of element and class nesting makes customisation a nightmare. I don't fancy nightmares so out bootstrap goes.

Well, I do need a css framework though, and I am really enjoying [Bulma]() despite it being in alpha or beta -- or some other greek letter -- version. Basically it is a very young framework, but it is mighty powerful!

So the first thing to note about Bulma is that it uses Sass files. You need to tell Rails that the Sass content needs to be preprocessed before being acquired. But there is a catch with the way Rails' [asset pipeline](http://guides.rubyonrails.org/asset_pipeline.html) behaves which I elaborate upon later.

Here are the steps I used to integrate the Bulma framework into the Rails app:

* `cd` into `app/assets/stylesheets`
* run `npm install bulma`

(I ran `tree node_modules/ -L 2 --dirsfirst` inside `/app/assets/stylesheets` to show you this lovely tree of the directory structure)

{% highlight text %}
node_modules/
└── bulma
    ├── css
    ├── sass << move this
    ├── bulma.sass << move this
    ├── CHANGELOG.md
    ├── LICENSE
    ├── package.json
    └── README.md
{% endhighlight %}

Move the `sass` folder and the `bulma.sass` file into stylesheets level - the level above the node_modules folder. Then delete the nodule_modules folder entirely. 

Trying to run the rails app at this point should throw an error where Rails doesn't know what to do with a variable like `$background: #eee` when rendering the CSS.

This is because the manifest file looks like this now:

```css
/*
 * This is a manifest file that'll be compiled into application.css, which will include all the files
 * listed below.
 *
 .
 .
 .
 *
 *= require_self
 *= require_tree .
 */
```

It would seem that Rails doesn't process any language that requires preprocessing unless it:

1. Is `@import`ed into a Sass file
2. Required in the manifest file

This means that we need to change the manifest file so that only a "master" sass file that imports all other sass files is mentioned:

```css
/*
 * This is a manifest file that'll be compiled into application.css, which will include all the files
 * listed below.
 *
 .
 .
 .
 *
 *= require_self
 *= require bulma
 */
```

However this means that we would have to keep all the css within the bulma sass file (which would be difficult for maintenance later on) or keep adding `*= require sass_file_name` to the manifest file for each sass file dedicated to a resource such as `users.sass`.

The best way to go about this is to import the current tree using `@import '*'`. You can read about it [here](http://guides.rubyonrails.org/asset_pipeline.html#manifest-files-and-directives):

```css
/*! bulma.io v0.2.3 | MIT License | github.com/jgthms/bulma */
@charset "utf-8"

@import "sass/utilities/_all"
@import "sass/base/_all"
@import "sass/elements/_all"
@import "sass/components/_all"
@import "sass/grid/_all"
@import "sass/layout/_all"
@import "*"
```

Pretty sweet. This is how you would go about implementing a framework into your Rails app without using gems.

Next up... I will have a think about the Users and Posts resources.