---
layout: post-show
title: Filling Forms
keywords--0: Filling Forms
title--before: 
keywords--1: 
title--middle: 
keywords--2: 
title--after: 
---

I have always thought filling out forms was very tedious - but I had yet to write the darn things. 

Why and how do we write forms on the web the way we do?

Okay, so forms enable servers to receive input from users. When the user hits the submit button, data within the form gets shipped to some location through another HTTP request. 

Where the form gets shipped to is generally on the same page. The method associated with that request will determine how the form is interpreted. The methods involved are usually GET, POST or PUT/PATCH.

### GET vs. POST methods for Forms:

Shipping forms via a GET request typically occurs for search engines. A GET request suggests that the information is not sensitive. For example on google, if we were to perform a search on 'hello world' what we are actually doing is submitting 'hello world' in a text field of a form. This input contains the name attribute 'q' (short for query) which is similar to a variable assignment. The corresponding URL for submitting a 'hello world' search would be: `https://www.google.co.nz/#q=hello+world`

{% highlight html %}
# The bare bones of the google form
<form action='www.google.co.nz' method='GET'>
  <label for="q">
  <input name="q" type="text">
</form>
{% endhighlight %}

On the contrary, shipping forms via a POST request suggests that the information is sensitive. A familiar example would be signing up or logging in to your account. So where is the information in this form shipped to when you hit submit? The clue is that a fresh HTTP request is __always sent__ when you hit the submit button. The next place to look at in your Rails app is then your `route.rb` file which directs the HTTP request and URL path pair to a controller and action pair!

So for a Sign Up form:
{% highlight html %}
<form action="/signup" method="post">
  <label for="user[username]">Username:</label>
  <input type="text" name="user[username]" value="">
  
  <label for="user[email]">Email:</label>
  <input type="text" name="user[email]" value="">
  
  <label for="user[password]">Password:</label>
  <input type="password" name="user[password]" value="">
  
  <input type="submit" value="Submit">
</form>
{% endhighlight %}

You would then match this form up to the `post '/signup'` route that points to the `users#create` controller and action:

{% highlight ruby %}
Rails.application.routes.draw do
  get  '/signup',  to: 'users#new'
  post '/signup',  to: 'users#create'
end
{% endhighlight %}

### POST for persistent data:

Unlike a search query though, the whole purpose of signing up is to make your form submission persist. So the data sent within the form must be stored within a persistent data structure - a database! 

Now the database is like a lake of pure water that's staying pure - if anything is going into this lake it has got to be pure water. Therefore there are certain validations present to make sure the contents of the form fit our criteria and these work at a browser javascript (very unsecure), a model level (catches most things) and at a database level (most secure). However this is not the first checkpoint for validation. In fact before these validations occur, the server needs to protect itself from cross-site request forgery attacks and prevent malicious scripts from being submitted.

To protect the form from cross-site request forgery, the form needs to be submitted with an __authentication token__ which Rails generates for us (note the type attribute is set to hidden):

{% highlight erb %}
<input type="hidden" name="authenticity_token" value="<%= form_authenticity_token %>">
{% endhighlight %}

Whenever a form is available, this serves as an entry point for an external entity to enter and cause internal damage. Even though there are finite input fields at the front end of the application, under the hood anyone is still able to modify the form and submit any input field with malicious content inside if they wanted to. To prevent this from occurring, you would have to use __strong parameters__ which means that the server will only accept certain input fields. 

Information within the form is stored and accessed within the `params` hash. The name attribute within the input element will serve as the key while the value attribute will serve as the value (the contents of the blank field you fill out gets appended to the value attribute).

Without strong parameters you would access the following form input with `params[:email]`:

{% highlight erb %}
<input type="text" name="email" value="cornelius.soon@gmail.com">
{% endhighlight %}

With strong parameters, you must also include the context of the data model you're trying to save this data to which in this example is a user. The following input would be accessed with `params[:user][:email]`:

{% highlight erb %}
<input type="text" name="user[email]" value="cornelius.soon@gmail.com">
{% endhighlight %}

### Don't Repeat Yourself:

I also forgot you need to set a form's `accept-charset` attribute to a character set so the browser knows how to interpret characters:

{% highlight html %}
<form accept-charset="UTF-8" action="/signup" method="post">
{% endhighlight %}

But this is one heck of a lot of things to remember just to set up a form:

1. Accept UTF-8 characters only
2. Use method POST (majority of the time)
3. Set an authentication token
4. Set up strong parameters

Rails acknowledges that this often what you desire as a web developer so the metaprogramming is set up to achieve this by hiding a lot of things:

{% highlight erb %}
<%= form_for(@user, url: users_path) do |f| %>
  <%= f.label :username, 'Username:' %>
  <%= f.text_field :username %>
  
  <%= f.label :email, 'Email:' %>
  <%= f.text_field :email %>
  
  <%= f.label :password, 'Password:' %>
  <%= f.password_field :password %>
  
  <%= f.submit 'Submit' %>
<% end %>
{% endhighlight %}

1. <s>Accept UTF-8 characters only</s>
2. <s>Use method POST (majority of the time)</s>
3. <s>Set an authentication token</s>
4. <s>Set up strong parameters</s>

All of these features are __implicitly__ implemented with the `form_for` helper that is established in Rails.

Most of the features made implicit are quite straight-forward bar the strong parameters. To set up the strong parameters you would have to set up your corresponding controller file like this:

{% highlight ruby %}
class UsersController < ApplicationController
  def create
    @user = User.new(user_params)
    if @user.save
      redirect_to user_url(@user)
    else
      render :new
    end
  end
  
  private

  def user_params
    params.require(:user).permit(:username, :email, :password)
  end
end
{% endhighlight %}

Which basically shortens:

{% highlight ruby %}
@user = User.new(params[:user][:username], 
                 params[:user][:email], 
                 params[:user][:password],
                 params[:user][:password_confirmation])
{% endhighlight %}

To:

{% highlight ruby %}
@user = User.new(user_params)
{% endhighlight %}

There's still a lot of under the hood stuff going on but... at least I have dusted off a few concepts.

<br>

### Happy [Hacktoberfest](https://hacktoberfest.digitalocean.com/)!