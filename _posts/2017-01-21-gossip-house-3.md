---
layout: post-show
title: 
keywords--0: Gossip House - Testing Sign Up
title--before: (Part III)
keywords--1: 
title--middle: 
keywords--2: 
title--after: 
---

Find Part II [here]({% post_url 2016-11-18-gossip-house-2 %})

Transferring the liability of errors inherent in the task of writing source code from user to machine is accomplished through automated testing. Used purposefully, test driven development makes writing source code much more enjoyable and pleasurable.

Of course, testing, is not without errors. Test code that is not written well could very well be testing the wrong thing or nothing at all. In this manner, testing incorrectly can give off the illusion that the source code is both functional and reliable when it isn't.

To understand whether the test code is achieving its intended target, nothing supercedes following a passing test with manual testing. This approach helps the operator to begin interpreting the relationship between automated and manual testing. Consequently, one not only appreciates how automated testing replaces manual testing, but also how to write them. Knowledge of how to write these tests reflect principles that we have unknowningly picked up and incoporated into our understanding. It is these silent and unheard principles which I would like to elaborate upon since they will save newcomers the expense of much frustration and disappointment associated with a trial-and-error approach to understanding testing.

Yes, an understanding of these principles will turn the process of testing from one of moritification to one of delight (or at the very least, tolerance).

### First: manual vs automated testing

If manual testing is having a user physically verify each element of the web application, whether it be link or content, checking for functionality etc, then automated testing is having a "command line" version of that. This is the key: after grasping this one core principle, testing becomes much more intuitive to understand.

Where one would change directories in a terminal for shell to interpret, you too would require valid commands to be inputted in order to both _get_ to the correct webpage _and test_ it.

### Second: the commands

Like CLI commands, one must understand where one initially starts and where one is headed towards.

Where exactly are we? Well... it's kind of complex. On a superficial level, we are located as one would be when one fires up a web browser with a blank homepage url. However it becomes obvious when one inspects his surroundings that he is actually inside the test environment. 

The 'test environment' is an abstract and desolate place with scatterings of small populations throughout. These populations reflect copies or certain mirages of structures we have worked with inside the development environment. These structures include our database, hypertext markup files, cascading stylesheets and rail helper files etc.

It then becomes the tester's job to connect these discrete structures or populations (while the metaphor sustains) together to see if they are actually behaving as expected.

Following is a single integration test block that is helpful for illustrating the scope of the test-environment. For context, this test is testing for a conventional sign-up on a website:

```ruby
# app/test/integration/users_sign_up_test.rb
require 'test_helper'

class UsersSignUpTest < ActionDispatch::IntegrationTest

  test "sign up with invalid information" do
    get sign_up_path
    
    assert_no_difference 'User.count' do
      post users_path, params: { user: { username: "hop",
                                         email: "hop@govt!",
                                         password: "",
                                         password_confirmation: "" }}
    end
    
    assert_template 'users/new'
    assert_select 'form[action="/users"]'
    assert_select 'div#error_explanation'
    assert_select 'div.field_with_errors'
  end
end
```

Here's the breakdown:

```ruby
# we are inside the test environment:

class UsersSignUpTest < ActionDispatch::IntegrationTest
  #
  #
  #
end
```

```ruby
# we open the web browser

class UsersSignUpTest < ActionDispatch::IntegrationTest
  test "sign up with invalid information" do
    #
    #
    #
  end
end
```

```ruby
# we enter the url for the sign-up page
# we send a HTTP GET request for the sign-up page

class UsersSignUpTest < ActionDispatch::IntegrationTest
  test "sign up with invalid information" do
    get sign_up_path
  end
end
```

```ruby
# we enter strings within the input fields and click submit
# we send a HTTP POST request to the users url with these form parameters

class UsersSignUpTest < ActionDispatch::IntegrationTest
  test "sign up with invalid information" do
    get sign_up_path
    
    post users_path, params: { user: { username: "hop",
                                       email: "hop@govt!",
                                       password: "",
                                       password_confirmation: "" }}
  end
end
```

This very point is a decisive moment of the coding process. Do you know what this test achieves?

'Absolutely nothing' is close, but would be incorrect. The test block is certainly not verifying anything (keyword: verify) but it is definitely navigating somewhere. The direction of our navigation is determined by the '[request helper methods](http://guides.rubyonrails.org/testing.html#available-request-types-for-functional-tests)'.

The key word (that doubles up as a method) for making a test block verify elements of our web application is *assert*. When we make an assertion, we make verifications in order to assert that something is true - otherwise it is false.

Back to the test block:

```ruby
# our first assertion!
# we assert that our User model, which is mapped to our users database does not change in its number of records after submission of a form with those parameters

class UsersSignUpTest < ActionDispatch::IntegrationTest
  test "sign up with invalid information" do
    get sign_up_path
  
    assert_no_difference 'User.count' do
      post users_path, params: { user: { username: "hop",
                                         email: "hop@govt!",
                                         password: "",
                                         password_confirmation: "" }}
    end
  end
end
```

Another checkpoint to discuss: this is the first mention of another structure in our test environment - ie, a *database*. This is important to keep in mind because the test database is separate from the development and production environments. The test database therefore contains fake, pre-generated data which you must specify for test to work out of!

There is in fact, another structure that has been ommitted for awhile which is `require 'test_helper'`. This helper file is a very unique structure because it entails the beginnings of pulling in more remote structures like gems/libraries to help improve our testing or to populate virtual entities. One example of this 'virtual' quality (albeit that everyhing in software is already virtual) that you can find in Hartl's tutorial is how you will have to generate methods within `test_helper` to mirror a login because the sessions hash does not work inside a test environment directly...

Now for the final rundown:

```ruby
# pulling in remote objects..
# populating environemnt...

require 'test_helper'

class UsersSignUpTest < ActionDispatch::IntegrationTest
  test "sign up with invalid information" do
    #
    #
    #
  end
end
```

```ruby
# Another assertion - this assertion points elsewhere.
# This assertion is looking towards Views instead of the Models as seen in assert_no_difference

require 'test_helper'

class UsersSignUpTest < ActionDispatch::IntegrationTest
  test "sign up with invalid information" do
    get sign_up_path
    
    assert_no_difference 'User.count' do
      post users_path, params: { user: { username: "hop",
                                         email: "hop@govt!",
                                         password: "",
                                         password_confirmation: "" }}
    end
    
    assert_template 'users/new'
  end
end
```

```ruby
# assert_select looks at html elements directly
# this assertion tests for the front-end interface of rendered webpages - this means it can potentially test for back-end data being plugged in!

require 'test_helper'

class UsersSignUpTest < ActionDispatch::IntegrationTest
  test "sign up with invalid information" do
    get sign_up_path
    
    assert_no_difference 'User.count' do
      post users_path, params: { user: { username: "hop",
                                         email: "hop@govt!",
                                         password: "",
                                         password_confirmation: "" }}
    end
    
    assert_template 'users/new'
    assert_select 'form[action="/users"]'
    assert_select 'div#error_explanation'
    assert_select 'div.field_with_errors'
  end
end
```

### Summary

Using automated testing in place of manual testing is not without errors. The first call of action is then to understand how to visualise automated testing which very closely resembles a command line interface. Instead of verifying by seeing with our eyes or through multiple clickings, we rely on assertions within the test. However, exactly like visiting a web page to verify content, we must first navigate and make travels in our test-environment via request methods before we can make assertions.