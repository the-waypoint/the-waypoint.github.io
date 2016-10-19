---
layout: post-show
title: Understanding Frameworks
keywords--0: 
title--before: The 
keywords--1: Subway
title--middle: Metaphor
keywords--2:
title--after: 
---

Scaffolding is the quick-fire way for a Rails app to set up an interface for managing RESTful urls. This interface is largely dependent upon the router and a resource-specific controller _passing on_ the message of a HTTP request. So where and how are things connecting with each other?

If your rails framework is a subway system, then the `rails generate scaffold` command _builds_ train tracks within your subway:

Imagine the vast labyrinth of an underground subway full of winding tracks going to different places. Your job as a developer is to be able to visualise what paths each train is supposed to take. Luckily you're on the outside holding a map in your hands: you've got a top-down blueprint view of the subway system with all its various paths! But... as all the paths cross each other and there is no clear direction of where any of them are headed, you still have no idea which path even one train takes to make a full circuit.

You see, the train represents a transport that travels from one station to another; its intent doesn't actually change - it's a __vehicle for messages.__ Whereas the passenger, that get on and off on the train, are the messages. Each message gets on and off at different stations - that is, at different points of the MVC architecture - with the aim of moving a payload of data across:

{% highlight shell %}
Subway  == Rails App          Station 1 == Router
Train   == Vehicle            Station 2 == Controller
People  == Messages           Station 3 == Model
Payload == Data               Station 4 == View

The Train enters the Subway through Station 1:
then    --> Station 2
then    --> Station 3*
back to --> Station 2 
then    --> Station 4

* where a secret (SQL) tunnel leads to a database
{% endhighlight %}

The following command should be familiar - it creates a scaffold for the Users resource with the name and email attributes:

{% highlight shell %}
rails generate scaffold Users name:string email:string
{% endhighlight %}

When this `scaffold` command is run, <u>all four stations</u> are set up for RESTful trains to access the 'Users resource'. In fact, the subway is expecting **7 types** of RESTful trains to run a 'Users resource' payload:

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
       database<a href="#1"><sup>[1]</sup></a></td>
    </tr>
  </tbody>
</table>
</div>
</article>

<div  class="table__div--footer">
  <p><a href="https://www.railstutorial.org/book/toy_app#table-demo_RESTful_users"><b>Table 2.2</b></a> adapted from Hartl</p>
  <p><a name="1">[1]</a> The database still retains information that it once stored an entry within this row numbered :id. This is why you despite deleting an object and creating "space" for a new one, the new object will only fill the next available :id space in the database. This is intentional for security reasons.</p>
</div>


The `scaffold` sets up a lot of tracks and stations for the train to use. This is achieved through a combination of placing new files in certain locations and placing written methods within those files. I use the verb 'placing' because under the hood, through much metaprogramming, Rails has already prescribed a lot of the code connecting and managing these pathways. As a developer, *you get to do the fun part of __plugging__ everything together*.

A lot of 'Users resource' related files will populate the Rails app in order to achieve this setup. As a beginner you should sear the following project tree and flow diagram into your mind:

{% highlight text %}
--app
  |--controllers
  |  |users_controller.rb     ------------- Station 2 ---------------
  |                               ▲          ▼     ▲         ▼
  |--models                       ▲          ▼     ▲         ▼
  |  |user.rb                     ▲         Station 3        ▼
  |                               ▲                          ▼
  |--views                        ▲                          ▼
     |--users                     ▲                    --------------
        |index.html.erb           ▲                                
        |show.html.erb            ▲                      Station 4  
        |new.html.erb             ▲                                
        |edit.html.erb            ▲                    --------------
--config                          ▲
  |routes.rb                  Station 1
{% endhighlight %}

\*\*\* A wild RESTful `GET domain.com/users` train appears! \*\*\*
{: style="font-weight: bold; text-align: center;"}

All right, you are not going to fight this train and send it to a hospital like some crazy pokemon trainer, but you are certainly gonna track it. By tracking how the train moves from one location to another you will hopefully be able to see a __pattern__ emerging for how the vehicle transfers messages for us:
* The train enters the Subway (your Rails App).
* The train heads for Station 1 (`routes.rb`) and drops off a `GET /users` HTTP request message.
* The train receives a message called (`users#index`) and heads for Station 2 (`users.controller.rb`) expecting an `index` message.
* On arrival to Station 2, the train receives the `index` message. The message reads out instructions on how to find the appropriate payload at Station 3. (`user.rb`)
* Station 3 receives the train and returns the requested payload<sup>[\[2\]](#2)</sup> as documented in the `index` message.
* The train loads the entire payload at Station 3 and then returns back towards Station 2.
* When the train arrives at Station 2, the payload is packaged into more discrete containers so that it can be delivered to different sectors of Station 4 (`index.html.erb`).
* Before leaving for Station 4, the train receives its last messages - these final messages allow Station 4 to unload the containers and plug in the data of the payload anywhere.
* At Station 4, data from your payload is integrated into HTML and a webpage is constructed.
* Finally, the train exits the subway and a webpage is delivered to the world.<sup>[\[3\]](#3)</sup> The train explodes into a million bits.

<div  class="table__div--footer">
  <p><a name="2">[2]</a> Technically messages are passed to and fro between the model and the database through a secret (sql) tunnel in order to retrieve the correct payload. But for brevity and to minimise confusion, this has been ommitted.</p>
  <p><a name="3">[3]</a> Some diagrams show the train reaching Station 4, picking up the webpage and then heading back to Station 2 before serving the webpage. Whatever the case may be, the important point is that you have understood how many stations the train has stopped at once it has entered the subway.</p>
</div>

### How are the stations set up?

If you do some quick maths - there are 7 RESTful trains and 4 stations for each so you would expect `7 * 4 == 28` stations to be present but this isn't the case.

{% highlight ruby %}
#./config/routes.rb

Rails.application.routes.draw do
  resources :users
  root 'users#index'
end
{% endhighlight %}

Can you describe what happens when...

\*\*\* A wild RESTful `GET domain.com/users/:id` train appears \*\*\* ?
{: style="font-weight: bold; text-align: center;"}

or...

\*\*\* A wild RESTful `GET domain.com/users/new` train appears \*\*\* ?
{: style="font-weight: bold; text-align: center;"}

[![If Pokemon were animals](http://cdn.bulbagarden.net/upload/3/3c/Pok%C3%A9mon_Center_Chansey.png)](https://www.youtube.com/watch?v=KNr8yM9CUYc)
{: style="text-align: center;"}