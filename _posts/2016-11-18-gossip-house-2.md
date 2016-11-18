---
layout: post-show
title: 
keywords--0: Gossip House - Sign Up
title--before: (Part II)
keywords--1: 
title--middle: 
keywords--2: 
title--after: 
---

Find Part I [here]({% post_url 2016-11-17-gossip-house-1 %})

I found that just starting this Gossip House Rails app was super difficult. There are though, a few principles that gave me a boost:

* [Roll out one feature](https://www.quora.com/Ruby-on-Rails-web-framework/Whats-an-efficient-web-app-development-process) at a time. An app is about functionality. This is achieved through vertical — not horizontal — integration. It's a depth-first approach.
* Make the blueprints for your app. Outline what you are trying to achieve. I draw inspiration for this development process through Matthew Inman's post on [how he built mingle2](http://mingle2.com/blog/view/how-i-built-mingle2).
* "The ones that win are the ones that ship." - is a quote I remember reading from [Dive Into Html 5](http://diveintohtml5.info/past.html). It is a very cool resource about the history of HTML5 and outlines why she is the way she is. Its arcane preface may cause the unawares to overlook its value: don't! It's such a valuable resource to learn from.

<br>

So after having a think about the resources, I determined that Gossip House was simply an app with Users, Posts and authentication. I then drew a mind-map for all the features I required.

Here's a short listing of the plan:

* Users --> have many posts
  * __sign up__
  * __log in__
  * authenticate (extras!!)
    * activated?
    * password reset
  
* Posts --> belongs to user
  * __new post__
    * post to database
    * get id of current user logged in and save as foreign key SQL
  * __show all posts__
    * each posts have username and message
    * however usernames are not visible if not logged in; this involves something like `if current_user == nil`, `name = "???"`; `else` `posts.user_id.name`

<br>

Well, a web app isn't very fun without users so off I went to make the first feature: __sign up__.

From a data point of view, the premise of the sign up feature is about getting new users into the database via the front end. This is the key principle underpinning my workflow. It keeps my compass needle poininting in the right direction whenever I am lost with all the method calls and MVC architecture.

First of is a scaffold:

```shell
rails g scaffold username:string email:string password_digest:string
```

Initially though my scaffold was generated using:

```shell
rails g scaffold username:string email:string password:string password_confirmation:string
```

However for important security measures the former supercedes the latter. I will elaborate later but it's becoming clear that back end development always come back to data!

<blockquote>
Data is power, guard it well.
</blockquote>

I then inspect my config/routes.rb file and set my root url through the syntax `root 'controller#action'` as `root 'users#index'`. This follows REST url convention. I set the root different to `users#new` in anticipation for a redirect and flash message.

<br>

I actually ended up deleting all my scaffold generator methods to start fresh. Although I did undo the deletes a couple of times to refresh my memory on how the controller actions were supposed to work again (doh).

I initially set up the `index` and `new` actions for now: 

```ruby
class UsersController < ApplicationController
  
  def index
    @users = User.all
  end
  
  def new
    @user = User.new
  end
  
end
```

I am though, in the back of my mind, anticipating the `create` action which necessarily follows a form submission from the `new` view. Trying to set the `create` action now though isn't following the "flow" of the app. So I avoid it for now.

<br>

If I am going to be making a new user, I have to ensure that there are at least some validations of the data being passed:

The syntax for validations follows:

```ruby
validates :attribute, validation: arguments
```

I want to make validations for `:username`, `:email`. `:password` and `:password_confirmation`. Instead of writing out the validations for the latter two, I instead use the helper method `has_secure_password` which is supplied through [Active Model](http://apidock.com/rails/v4.0.2/ActiveModel/SecurePassword/ClassMethods/has_secure_password) and [bcrypt](https://github.com/codahale/bcrypt-ruby).

`has_secure_password` is a helper method that supercedes the `password` and `password_confirmation` validations in the User model:

* It ensures that both `password` and `password_confirmation` attributes are actually present. Bcrypt does this by creating _virtual attributes_ to work with.
* It also ensures that both of these attributes match.
* It then saves this password as a digest within the database which makes it far more secure than a naked password.

NB: the password is NOT encrypted - it's digested. The difference is that encryption is a reversible action. A password that is encrypted can be decrypted. A digest however, only goes one way. Once you digest a password it can only become poop - that is, a string of [unique gibberish](https://github.com/codahale/bcrypt-ruby). Long story short, trying to find out the original password of a digested password is far harder to do than an encrypted password.

To include the functionality of digests in the rails app, I include `gem 'bcrypt'` in my Gemfile and then run `bundle install` again.

My model file ended up looking something like:

```ruby
class User < ApplicationRecord
  
  VALID_EMAIL_REGEX = /\A([\w+\-].?)+@[a-z\d\-]+(\.[a-z]+)*\.[a-z]+\z/i
  
  validates :username, presence: true,
                       length:   { in: 6..50 }
  validates :email,    presence: true,
                       uniqueness: true,
                       format: { with: VALID_EMAIL_REGEX, message: "is not valid" }
  has_secure_password
  validates :password, length: { in: 6..250 }
end
```

<br>

Now that I had the validations in place. It was time to get a 'users#new' form sorted. A lot of it was already generated in the scaffold command via a `form_for` helper in the views. The way the scaffold sets it up is to render a partial in the `new.html.erb` view which can be reused in the `edit.html.erb` view.

However, the form was damn ugly. UGLY. 

So I needed to insert a few classes in order for the form to appreciate the Bulma CSS framework I used. Trying to insert arguments into the form helper methods threw heaps of errors. This was because each form helper — assigned to a different type of element and understandably holding different attributes — accepted the arguments differently. So what really helped was having a look at the Rails API docs to see a list of all the Ruby on Rails helpers used in views. There was a [convenient answer](nhttp://stackoverflow.com/questions/3779100/is-there-a-list-of-all-ror-helpers-used-in-view-pages) on Stack Overflow which demonstrated that all these methods were called via `ActionView::Helpers`. From there it was easy looking up [how to include classes](http://api.rubyonrails.org/classes/ActionView/Helpers/FormHelper.html#method-i-form_for) in the helper methods.

The styling helped a lot however the error messages were being placed in a very jarring manner. What I wanted was for only the relevant error messages to be beneath the corresponding input fields. What transpired was code that isn't DRY but works. That's fine, I'll refactor it later.

BEFORE - where the initial code which smushed all the errors together:

{% highlight erb %}
<% if user.errors.any? %>
  <div id="error_explanation">
    <h2 class="notification is-danger">
      <%= pluralize(user.errors.count, "error") %> prevented you from signing up
    </h2>

    <ul class="help is-danger">
    <% user.errors.full_messages.each do |message| %>
      <li><%= message %></li>
    <% end %>
    </ul>
  </div>
<% end %>
{% endhighlight %}


AFTER - where error messages only appear under their respective input field:

{% highlight erb %}
<% if user.errors.any? %>
  <div id="error_explanation">
    <h2 class="notification is-danger">
      <%= pluralize(user.errors.count, "error") %> prevented you from signing up
    </h2>
  </div>
<% end %>

<div class="field control">
  <%= f.label :username, class: "label" %>
  <%= f.text_field :username, class: "input" %>
  
  <% user.errors.messages[:username].each do |message| %>
    <li class=" help is-danger">
      <%= "Username #{message}" %>
    </li>
  <% end %>
</div>

<div class="field control">
  <%= f.label :email, class: "label" %>
  <%= f.text_field :email, class: "input" %>
  
  <% user.errors.messages[:email].each do |message| %>
    <li class=" help is-danger">
      <%= "Email #{message}" %>
    </li>
  <% end %>  
</div>
.
.
.
{% endhighlight %}

<br>

Now that the form is set up, the request it sends is a `post` to `users_path` which is routed towards the `users#create` action. Since parameters are being passed I need to ensure they are [whitelisted](http://edgeapi.rubyonrails.org/files/actionpack/lib/action_controller/metal/strong_parameters_rb.html). This is a [security measure](https://insoshi.wordpress.com/2008/09/21/mass-assignment-in-rails-applications/) to prevent __mass assignment__ of an object which can compromise the database:

```ruby
class UsersController < ApplicationController
  .
  .
  .
  def create
    @user = User.new(user_params)
    if @user.save
      redirect_to root_url
      flash[:success] = "You have successfully signed up #{@user.username}!"
    else
      render 'new'
    end
  end
  
  private
  
    def user_params
      params.require(:user).permit(:username, 
                                   :email, 
                                   :password, 
                                   :password_confirmation)
    end
  
end
```

Without strong parameters, creating a new user would be both insecure and tedious:

```ruby
@user = User.new(username: params[:username], email: params[:email], password: params[:password], password_confirmation: params[:password_confirmation])
```

With strong parameters, creating a new user would be secure but still tedious:

```ruby
@user = User.new(username: params[:user][:username], email: params[:user][:email], password: params[:user][:password], password_confirmation: params[:user][:password_confirmation])
```

With the private method `user_params` and the `.require` and `.permit` helper methods, creating a new user with strong parameters is finally both secure and efficient:

```ruby
@user = User.new(user_params)

def user_params
  params.require(:user).permit(:username, 
                               :email, 
                               :password, 
                               :password_confirmation)
end
```

<br>

After tidying up the flash message to show a successful sign up, I considered the sign up feature complete. Although... I still need to do something testing... :scream: