---
layout: post-show
title: rails generate scaffold
keywords--0: 
title--before: rails generate scaffold
keywords--1:
title--middle: 
keywords--2:
title--after: 
---

Frameworks are created with the premise of being lazy. They are purposed to prevent developers from rewriting the same code again and again for a particular task. Without frameworks, one would be wasting time trying to figure out the best principles for approaching their particular task when many have already been figured out.

The predicament for a new user to the Rails framework then is: how do I navigate all these abstract concepts that have been established before me?

Introduce: scaffold.

In Rails, the `scaffold` command implements a unit framework within a framework. Confusing? Let me explain:

Rails is a collection of Ruby scripts used to create an environment for shipping web applications. What the Rails framework establishes is an MVC architecture for dealing with incoming HTTP requests:

1. The browser issues a HTTP request for a resource.
2. The HTTP request is received by the **router** which finds the corresponding controller for the resource being requested.
3. The **controller** then looks up the action that fulfills the intentions of the HTTP request.
  * The combination of both router and controller act as an <u>interface</u> to accommodate RESTful urls.
4. When the controller initiates the action, a series of conventional events take place which will involve a model, a database and a view.
  * The keyword here is <u>conventional</u>; by following how RESTful routes manage resources, we can expect data to be handled in a CRUD fashion.
5. The **model**, having received the action runs procedures to talk to a database to retrieve the desired data.
  * Within the model you can specify what kind of data comes in and goes out of the system.
  * You can think of the model as a librarian who looks up and stores data. The model also doubles up as a security officer who screens the data: accepting data when it meets the criteria or rejecting it when it doesn't and giving reasons why.
6. The database is a depot that stores all the information for a particular resource. 
  * One resource can "socialise" with another resource once the model connects them together: the model defines these relationships.
7. Once the model has found and pieced together all the requested data. This data is sent back to the controller ready to be injected into the view.
8. The **view** acts as your standard HTML file but incorporates Ruby logic too. This is the game changer - the combination of all the previously described steps amounts to granting you the ability to present information to your audience dynamically, via automation.

**NB:** bold items indicate the names ruby files parsing information, "talking" to each other and then "handing" over information.

![MVC architecture](https://softcover.s3.amazonaws.com/636/ruby_on_rails_tutorial_4th_edition/images/figures/mvc_detailed.png)

Source: [https://www.railstutorial.org/book/toy_app](https://www.railstutorial.org/book/toy_app)

<br>
Using [Chapter 2 of Hartl](https://www.railstutorial.org/book/toy_app), lets fill in each numbered step with direct examples:

1. `GET root_domain/users` is entered in the browser and received by the router.
2. `routes.rb` parses this request and identifies `/users` as analogous to the User resource. The router talks to the users controller.
3. `users_controller.rb` identifies that the `index` action matches this HTTP request.
4. The `index` action initiates a series of events involving the user model, user database and users' view.
5. The `user.rb` model retrieves information about all users.
6. (A lot of SQL commands are entered here and are handled by the Object Relational Mapper called Active Record)
7. The model receives this data and prepares it for injection into the view.
8. The data is injected into `index.html.erb` by utilising Ruby logic within a HTML file! Data from the back end has been presented at the front end.

<br>
Now what does this all have to do with scaffold? The answer is essentially existential:

The `rails generate scaffold` command creates a collection of files for a particular resource following the MVC architecture and RESTful url convention. This brings the Rails framework into a working existence. There is now a clear flow for how the framework operates which gives **you** the opportunity to learn how each file interacts with one another.

To bring it home, here's a breakdown of the command:

{% highlight shell %}
  rails generate scaffold Users name:string email:string
{% endhighlight %}

* `rails` refers to the program you want to run in the terminal (I think it's a shell execute?)
* `generate` is the rails command to create something; think about how `rails server` is the rails command to start a server up
* `scaffold` is an additional rails command or it may be an argument...
* `Users` refers to the name of the resource you are creating
* `name:` and `email:` are the attributes you wish to assign to the Users resource
* `string` refers to which data-type is entered within the attributes. Other attributes that I have come across include integer and text.

<br>

When a unit framework exists within a framework, we call that unit a **scaffold.**