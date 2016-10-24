---
layout: post-show
title: SQL databases are like airstrips
keywords--0: SQL databases
title--before: are like
keywords--1: airstrips
title--middle: 
keywords--2: 
title--after: 
---

At a very basic level, SQL databases are tables with an x and y axis that bear relationship to one another. Each column represents a property and each row represents an entry. 

Remarkably though, both 'entry' and 'property' are symbiotes: the properties don't have anything to apply or attach to without an entry; entries, on the other hand, cannot be described without properties:

{% highlight shell %}
# Properties existing without entries:
 ___________________________________
| property1 | property2 | property3 |
|-----------+-----------+-----------|
|     ∅     |     ∅     |     ∅     |
|-----------+-----------+-----------|
|     ∅     |     ∅     |     ∅     |
 -----------------------------------
{% endhighlight %}

{% highlight shell %}
# Entries existing without properties:
 ___________________________________
|     ∅     |     ∅     |     ∅     |
|-----------+-----------+-----------|
|   value   |   value   |   value   |
|-----------+-----------+-----------|
|   value   |   value   |   value   |
 -----------------------------------
{% endhighlight %}

{% highlight shell %}
# Values tie together properties(columns) and entries(rows) together:
 _____________________________
|        theodinproject       |
|-----+-----------+-----------|
| id  |   name    |   skills  |
|-----+-----------+-----------|
|  1  |   Kevin   |    Rails  |
|-----+-----------+-----------|
|  2  |   Cody    |    Ruby   |
|-----+-----------+-----------|
|  3  | Cornelius |    Ruby   |
------------------------------
{% endhighlight %}

We then use SQL, the structured query language, to retrieve sets of data from these relational databases:

{% highlight sql %}
SELECT name
FROM theodinproject
{% endhighlight %}

Retrieves the `name` column from the `theodinproject` table:

{% highlight shell %}
 ___________
|   name    |
|-----------|
|   Kevin   |
|-----------|
|   Cody    |
|-----------|
| Cornelius |
 -----------
{% endhighlight %}

On a single table, multiple columns are easy to get:

{% highlight sql %}
SELECT name, skills
FROM theodinproject
{% endhighlight %}

{% highlight shell %}
 ___________ ___________
|   name    |   skills  |
|-----------+-----------|
|   Kevin   |    Rails  |
|-----------+-----------|
|   Cody    |    Ruby   |
|-----------+-----------|
| Cornelius |    Ruby   |
------------------------
{% endhighlight %}

These examples are especially easy to grasp because we are gathering data on _all entries_ and the table returned by SQL retains almost full semblance of the original table. The difficult part then, is to visualise this table as a fluid container where parts change readily like a [Transformer](https://youtu.be/33FB-HAvdqY) would.

For example the following SQL command,
{% highlight sql %}
SELECT name, skills
FROM theodinproject
WHERE skills LIKE 'Ru%'
{% endhighlight %}

would produce a table only including with both the `name` and `skills` column but only entries for skills that starting with 'Ru':
{% highlight shell %}
# Bye Kevin (too good for us)
 ___________ ___________
|   name    |   skills  |
|-----------+-----------|
|   Cody    |    Ruby   |
|-----------+-----------|
| Cornelius |    Ruby   |
------------------------
{% endhighlight %}

Now although there are only two axes at work to represent the data here, the columns and rows have rearranged themselves from the initial table to match the query. How you interpret this table shuffling will determine how you visualuse more complex SQL commands.

The analogy I use to best visualise this table shuffling is to think of the database as an airstrip:

![Airstrip](https://upload.wikimedia.org/wikipedia/commons/e/e1/Landing_at_Zurich_International_Airport.jpg)

All the properties/columns are represented as airstrip lights:

{% highlight shell %}
  id      name      skills  
   ▼        ▼         ▼     
   ▼        ▼         ▼     
   ▼        ▼         ▼     
   ▼        ▼         ▼     
   ▼        ▼         ▼     
   ▼        ▼         ▼     
{% endhighlight %}

While the entries/rows are represented by plane landings (don't ask me why planes land tangentially to the airstrip lights... I'm trying my best to describe a SQL table):

{% highlight shell %}
  id      name      skills  
   1 ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ 
   
   2 ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ 
    
   3 ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ ➞ 
{% endhighlight %}

When I call:
{% highlight sql %}
SELECT name, skills
{% endhighlight %}

I set up the `name` and `skills` airstrip lights. They go on for as far as the eye can see:
{% highlight shell %}
 name      skills  
   ▼         ▼     
   ▼         ▼     
   ▼         ▼     
   ▼         ▼     
   ▼         ▼     
   ▼         ▼     
{% endhighlight %}

Then when I call:
{% highlight sql %}
SELECT name, skills
FROM theodinproject
{% endhighlight %}


I know that I am retrieving data from `theodinproject` airspace:
{% highlight shell %}
 _____________________________
|       theodinproject        |
|-----------------------------|
        name      skills  
          ▼         ▼     
          ▼         ▼     
          ▼         ▼     
          ▼         ▼     
          ▼         ▼     
          ▼         ▼     
{% endhighlight %}

Then when I call the final part of the query:
{% highlight sql %}
SELECT name, skills
FROM theodinproject
WHERE skills LIKE 'Ru%'
{% endhighlight %}

This sets up the number of possible landings on our airstrip. Each landing corresponds to one entry:

{% highlight shell %}

|-----------------------------|
        name      skills  
          ▼         ▼     
➞ ➞ ➞ Cody ➞ ➞  Ruby ➞ ➞ ➞ ➞ 
          ▼         ▼     
 ➞ ➞ Cornelius ➞ Ruby ➞ ➞ ➞ ➞ 
          ▼         ▼     
          ▼         ▼     
{% endhighlight %}

Which is really just a table:
{% highlight shell %}
 ___________ ___________
|   name    |   skills  |
|-----------+-----------|
|   Cody    |    Ruby   |
|-----------+-----------|
| Cornelius |    Ruby   |
------------------------
{% endhighlight %}

This analogy is really useful for me because it conveys how Ruby objects can be stored as persistent data. Those property columns are actually mirroring attributes declared with `attr_reader :id, :name, :skills`! And then each row entry conveys an instance of the class `TheOdinProject`:

{% highlight ruby %}
class TheOdinProject
  attr_reader :id, :name, :skills
  
  def initialize(args)
    @id = args[:id]
    @name = args[:name]
    @skills = args[:skills]
  end

end
{% endhighlight %}

<br>
Wonderful. Now everything in SQL is an object too.



[![Reinforcements have arrived](http://vignette2.wikia.nocookie.net/cnc/images/1/1f/TD_Airstrip.gif/revision/latest?cb=20140117090600)](https://www.youtube.com/watch?v=ggCCJ4zkIWM)
