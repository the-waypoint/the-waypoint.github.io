---
layout: post-show
title: BACK<< to the FRONT END
keywords--0: BACK<<
title--before: to the 
keywords--1: FRONT-END
title--middle: 
keywords--2:
title--after:
---

The primary focus of this debriefing is to keep track of how a Rails app parses a HTTP request in order to serve a webpage. What we are attempting to do is keep track of how this **message** is passed and received between MVC objects within a Rails app.

The project tree for a Rails app is *huge*. However with comforting adage of 'convention over configuration', there is an obvious flow in which files are activated. This follows the MVC architecture:

`./routes.rb`

`./app/controllers/users/users_controller.rb`

`./app/models/user.rb`

`./app/views/users/index.html.erb`


A lot of "magic" happens under the hood after running:

{% highlight shell %}
rails generate scaffold Users name:string email:string
{% endhighlight %}

When a scaffold is generated, a standardised base of files, following the flow of the MVC architecture, is generated. The keyword here is <u>flow</u>. Each file has a specific location path and name in order for data to be transferred and handled from one location to another. The default code inside each file are also predictable. If this were not the case then seeking out and managing the Users resource would be very convoluted. As the idiom goes "convention over configuration" - things are set up nicely; now we just need to learn how they are set up.

In an MVC architecture we have 4 centers involved: they operate along a spectrum of interpreting the request, grabbing the right data and then serving a webpage:
1. Router
2. Controller
3. Model
4. View

The combination of the router and controller form an interface for parsing HTTP requests.

When you enter this into the browser:
{% highlight http %}
www.domain_name/users
{% endhighlight %}

* The router sees the HTTP method and the uri, `GET` and `/users` respectively, for this request.

{% highlight ruby %}
#./routes.rb

Rails.application.routes.draw do
  get users_index_url
end
{% endhighlight %}

* The router identifies that the Users resource is being requested and calls on the Users controller.
* According to RESTful architecture, the corresponding controller action is called `index`.

{% highlight ruby %}
#./app/controllers/users/users_controller.rb

class UsersController < ApplicationController
  def index
    @users = User.all
  end
end
{% endhighlight %}

* Within the `index` method, all User objects are assigned to `@users`. 
  * Each User object holds two **attributes** each of a certain data type. 
  * These attributes were of course generated in the scaffold: <u>name</u> and <u>email</u>. 
  * Entries for these attributes capture the <u>string</u> data-type as they have been defined.
* The `index` action is invoked and this fires up the User model to get data for us:

{% highlight ruby %}
#./app/models/user.rb

class User < ApplicationRecord
  has_many :microposts
  validates :name, presence: true
  validates :email, presence: true
end
{% endhighlight %}

* The User model in this scenario does not have any specifications regarding giving out information about all User objects.
* However, the User model is still playing a vital role about data exchange and screening; out of interest, you should realise that it specifies:
  * a relationship to another resource - `has_many :microposts`
  * requirements for data being received at the front-end -  `validates ... presence: true`
* The model retrieves data from the database: SQL stuff is "bypassed" in Rails by using an Object Relational Mapper called ActiveRecord.
* Remember that this retrieved data is all stored within the `@user` instance variable within the Users controller - it's been a long ride hasn't it?
* The controller then gives the corresponding view access to this instance variable. Since the action within the controller was called `index`, an analogous name for the view is conventionally used - `index.html.erb`:

{% highlight erb %}
<!-- ./app/views/users/index.html.erb -->

<tbody>
  <% @users.each do |user| %>
    <tr>
      <td><%= user.name %></td>
      <td><%= user.email %></td>
      </tr>
  <% end %>
</tbody>
{% endhighlight %}

* This is simple ruby logic embedded into a html file; taking away all the cocoon of html and embedding tags reveals:

{% highlight ruby %}
#./app/views/users/index.html.erb

@users.each do |user|
  user.name
  user.email
end
{% endhighlight %}

* Calling `.name` and `.email` on User objects seems quite familiar doesn't it? It looks a lot like an `attr_reader` method to me.
* The HTTP request is finally parsed and the Rails app returns to the client a webpage underpinned by dynamic processing.

<br>
There you have it: this is how a HTTP request makes it **back to the front end.**

[![Back to the Future - I mean Front End](https://upload.wikimedia.org/wikipedia/en/d/db/Back_to_the_Future_%28time_travel_test%29_with_Michael_J._Fox_as_Marty_McFly.jpg)](https://www.youtube.com/watch?v=Q-CVfyoDxLI)
{: style="text-align: center;"}