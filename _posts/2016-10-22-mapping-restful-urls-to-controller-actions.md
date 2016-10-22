---
layout: post-show
title: Mapping RESTful urls to controller actions
keywords--0: Mapping
title--before: RESTful urls to
keywords--1: controller actions
title--middle:
keywords--2:
title--after: 
---

In its entirety, the MVC architecture is difficult to explore. This is especially true when many concepts and structures are being layered in order to serve a webpage for a simple GET request. Yes, in short, the Rails framework is heavy. The good news is that is completely possible to follow the flow at which [messages are transported](({% post_url 2016-10-20-a-wild-restful-train-appears %})) from one file to another.

For a simple navigation bar on your website comprising of links to `Home`, `About` and `Contact`, a website needs to serve a webpage for each when clicked on. At a basic level these links are considered 'static' or 'flat' because there's nothing required beyond `GET`ting that single webpage.<sup>[\[1\]](#1)</sup> Compare this `Navagation` resource to a `User` resource where you are requiring a RESTful architecture to manage forms with inputted details and integrating that data to serve a tangible instance of a new user - for example, think about account creation on GitHub.

<div class="table__div--footer">
  <p><a name="1">[1]</a> It is of course possible to have a more dynamic contact page with forms for sending a quick message.</p>
</div>  

On a full circuit, the 4 stations that are visited in the MVC architecture are:

{% highlight text %}
/config/routes.rb  

/app/controllers/pl_resource_controller.rb/

/app/model/s_resource.rb

/app/views/pl_resource/controller_action.html.erb

pl == 'pluralised'
s  == 'singular'
{% endhighlight %}

This full circuit is required in the `Users` resource because the premise of **data persistence** exists. If we don't make a trip to the model, we cannot save the data - there would be no back end to give this webpage its dynamic quality! In contrast to the `Navigation` resource, we don't need to save or serve any data so a trip to the model can be skipped. 

Another way to put this is that the controller actions in the 'Users' resource point towards the model before serving a view. While in the 'Navigation' resource, the controller actions point to a corresponding view.

For generating the 'Users' resource we would run a `scaffold` command which sets up all 4 stations in a RESTful manner:

{% highlight shell %}
rails generate scaffold Users name:string email:string
{% endhighlight %}

For generating the 'Navigation' resource we would run a `controller` command. This command only sets up 3 out of the 4 stations:

{% highlight shell %}
rails generate controller Navigation home about contact
{% endhighlight %}

which outputs:
{% highlight shell %}
route  get 'navigation/home'
route  get 'navigation/about'
route  get 'navigation/contact'

create  app/controllers/navigation_controller.rb
  
create    app/views/navigation
create    app/views/navigation/home.html.erb
create    app/views/navigation/about.html.erb
create    app/views/navigation/contact.html.erb
{% endhighlight %}

The output of `generate controller` sets up your routes, controller and views. Notice there is no model file:

{% highlight text %}
--app
  |--controllers
  |  |navigation_controller.rb  ------------- Station 2 ------------------
  |                               ▲                                ▼
  |--models                       ▲                                ▼
  |  |empty                       ▲           Station 3            ▼
  |                               ▲         (not visited)          ▼
  |--views                        ▲                                ▼
     |--navigation                ▲                          --------------
        |home.html.erb            ▲                                   
        |about.html.erb           ▲                            Station 4  
        |contact.html.erb         ▲                                
--config                          ▲                          --------------
  |routes.rb                  Station 1
{% endhighlight %}

The `scaffold` and `controller` commands don't only differ in the number of stations visited, but they also differ in the manner in which a HTTP request for the resource is routed.

When you open up the `config/routes.rb` file what you find is that all the RESTful routes for the 'Users' resource have been hidden under a neat helper method called `resources`. Whereas the routes for the 'Navigation' resource are made explicit:

{% highlight ruby %}
# /config/routes.rb

Rails.application.routes.draw do
  resources :users                  # implicit routes are **MAGIC!!**
  
  get 'navigation/home'             # explicit routes, less magic =(
  get 'navigation/about'
  get 'navigation/contact'
  
end
{% endhighlight %}

When you follow the `get 'navigation/about'` route to the various files and methods it connects to, you will find that these exact files and methods included are actually comprised from these pairs of words:

{% highlight shell%}
  navigation      /        about

 pl_resource      /   controller_action

{% endhighlight %}

## Lets begin tracking:

To cement how each file-path or method is linked to each other pay attention to:

1. Whether the resource is pluralised or singular.
2. What files are named after the resource or the controller action
3. What methods are named after the resource or controller action

{% highlight ruby %}
# Station 1
# /config/routes.rb

Rails.application.routes.draw do
  get 'navigation/contact'
end
{% endhighlight %}

{% highlight ruby %}
# Station 2
# /app/controllers/navigation_controller.rb

class NavigationController < ApplicationController
  def contact
  end
end
{% endhighlight %}

{% highlight ruby %}
# Station 3
{% endhighlight %}

{% highlight erb %}
# Station 4
# /app/views/navigation/contact.html.erb

<h1>Hello World my name is Cornelius!</h1>
{% endhighlight %}

Did you see the pattern?

Lets examine what `get 'navigation/contact'` involved:

- At Station 2:
  - the pluralised resource name is a prefix for the controller file path; this can be confusing since the word `navigation` itself is both the plural and the singular.
  - the `contact` action corresponds to method listed in the controller file

- At Station 4:
  - the pluralised resource name corresponds to the folder name holding its view files
  - the view file is named after the action.


You'll really know how to _navigate_ your project tree if you can fill in these blanks for the `get 'navigation/about'` or `get 'navigation/home'` route:

{% highlight text %}
/config/routes.rb  

/app/controllers/pl_FILL_controller.rb/
  . class pl_FILLController < ApplicationController
  .   def FILL
  .   end
  . end
  
/app/model/s_FILL.rb

/app/views/pl_FILL/FILL.html.erb

pl == 'pluralised'
s  == 'singular'
{% endhighlight %}

A simple rule to follow is that the naming of these file paths-and methods __pivot off__ that one simple line in your route file - `get 'navigation/about'`.

### Now for the RESTful routes:

If we try to track a RESTful route for the 'Users' resource we immediately run into two problems. The first is that we get stuck with a helper method that hides the pathing and makes it all implicit:

{% highlight ruby %}
# Station 1
# /config/routes.rb

Rails.application.routes.draw do
  resources :users
end
{% endhighlight %}

Well we know what the RESTful routes look like through this table:

<article>
<div class="overflow-x__div--scroll">
<table>
  <thead>
    <tr>
      <th>HTTP method</th>
      <th>URL</th>
      <th>Action</th>
      <th>Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>GET</td>
      <td>/users</td>
      <td>index</td>
      <td>get all users</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/users/:id</td>
      <td>show</td>
      <td>get user object labelled :id</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/users/new</td>
      <td>new</td>
      <td>get form for creating new user object</td>
    </tr>
    <tr>
      <td>GET</td>
      <td>/users/:id/edit</td>
      <td>edit</td>
      <td>get form for updating info on user object labelled :id</td>
    </tr>
    <tr>
      <td>POST</td>
      <td>/users</td>
      <td>create</td>
      <td>create a new user object; its :id is the next available row in your database</td>
    </tr>
    <tr>
      <td>PUT</td>
      <td>/users/:id</td>
      <td>update</td>
      <td>update a user object labelled :id with stuff</td>
    </tr>
    <tr>
      <td>DELETE</td>
      <td>/users/:id</td>
      <td>destroy</td>
      <td>destroy the user object labelled :id in the users     
       database</td>
    </tr>
  </tbody>
</table>
</div>
</article>

Alright so maybe the route file looks something like this:

{% highlight ruby %}
# Station 1
# /config/routes.rb

Rails.application.routes.draw do
  get    '/users'
  post   '/users'
  get    '/users/new'
  get    '/users/:id/edit'
  get    '/users/:id'
  patch  '/users/:id'
  put    '/users/:id'
  delete '/users/:id'
end
{% endhighlight %}

The second problem becomes apparent when you try to follow the `get /users` route. The reliable `pl_resource/controller_action` pair rule disappears and we must throw our previous assumptions out the window.

This is where the `to:` key becomes really handy. This connects the route back to the our `pl_resource/controller_action` pair. And once again we'll have all our file-paths and method names matching up again. 

{% highlight ruby %}
# Station 1
# /config/routes.rb

Rails.application.routes.draw do
  get    '/users',          to: 'users#index'
  post   '/users',          to: 'users#create'
  get    '/users/new',      to: 'users#new',    as: :new_user
  get    '/users/:id/edit', to: 'users#edit',   as: :edit_user
  get    '/users/:id',      to: 'users#show',   as: :user
  patch  '/users/:id',      to: 'users#update'
  put    '/users/:id',      to: 'users#update'
  delete '/users/:id',      to: 'users#destroy'
end
{% endhighlight %}

But I don't want to write out all those RESTful routes and then map them to a controller. So sticking with `resources :users` is a great idea. Now I'm not exactly sure what the implications are for mapping your routes to customised controller actions... but it has been a great exercise to understand how all these file paths are names are generated by first looking at the files created in  `rails generate controller` command and then compare it with the files created in a `rails generate scaffold` command. In any case, it's another interface of the Rails gem I have come to understand a lot better. 

Maybe an amazing Rails App using customised controller actions is waiting just round the corner? Who knows.