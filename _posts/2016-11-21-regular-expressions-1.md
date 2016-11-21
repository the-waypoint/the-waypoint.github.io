---
layout: post-show
title: 
keywords--0: Regular Expressions
title--before:
keywords--1: 
title--middle: 
keywords--2: 
title--after: 
---

Regular expressions will always continue to trip me up but I think I am finally get a grip on it.

I think it's really important to follow an in-depth guide for learning regex if you ever want to take it seriously. And the reason for this is that regex is to strings what BEDMAS is to math equations.

What I mean is that there are certain rules in precedence and convention that take place in choosing the most efficient (and least convoluted) regex for a string that are intuitively integrated into BEDMAS.

BEDMAS denotes the order of priority in which math events take place: brackets first, then exponents, division, multiplication, addition and subtraction last. In BEDMAS there are no surprises or subtleties left uncovered - everything has an order. In learning and applying regex though I tended towards stumbling and fumbling about with no clear direction before 'landing' on an answer. 

Even after acquiring an answer I would be no closer to understanding how regex worked. This is because, with each iteration of a regex, I would be analysing what the regex pattern would be achieving and then trying to work out the ruleset by finding a trend through reverse-engineering. This sort of logic however is erroneous because regex is very specific. The rules for matching in regex doesn't bend but when you apply it to a string, it gives off the illusion that the regex is working exactly as you perceived it to. Apply the same regex to a different string and the result is not only are there no matches found in the string you are checking but there are also no matches found in your poor brain. Trying to corroborate how regex works by trial and error without first learning first principles is wasted time and effort.

The most approachable way to appreciate regular expressions in Ruby is to use the [`#scan` method]() which returns all regex matches into an array for use. I initially tried using the [`#split` method]() on various exercises which is more focused on finding what the string isn't but they generally end up as failed attempts and noodle code so I will stick to `scan` now. After inspecting what the array looks like can we deconstruct regular expressions by first principle.

My list of first principles to know for regex are:

* Single character evaluation
* Brackets vs. Character Sets
* Matching
* Single Object Evaluation
* Modifiers

The string I like to use for analysis is an excerpt from Bob Dylan's poem 'Do Not Go Gentle Into That Good Night':

```ruby
phrase = "Do not go gentle into that good night\nOld age should burn and rave at close of day"
```

### Single Character Evaluation

Whenever you input a regex, if it is not delimited by repetitions or a range you are always evaluating by a single character:

```ruby
# Delimiter for a single alphanumeric character
phrase.scan(/\w/)
# => ["D", "o", "n", "o", "t", "g", "o", "g", "e", "n", "t", "l", "e", "i", "n", "t", "o", "t", "h", "a", "t", "g", "o", "o", "d", "n", "i", "g", "h", "t", "O", "l", "d", "a", "g", "e", "s", "h", "o", "u", "l", "d", "b", "u", "r", "n", "a", "n", "d", "r", "a", "v", "e", "a", "t", "c", "l", "o", "s", "e", "o", "f", "d", "a", "y"]


# Delimiter for an entire alphanumeric word
phrase.scan(/\w+/)
# => ["Do", "not", "go", "gentle", "into", "that", "good", "night", "Old", "age", "should", "burn", "and", "rave", "at", "close", "of", "day"]


# Delimiter for span of alphanumeric characters ranging from 1 to 3 in length
phrase.scan(/\w{1,3}/)
# => ["Do", "not", "go", "gen", "tle", "int", "o", "tha", "t", "goo", "d", "nig", "ht", "Old", "age", "sho", "uld", "bur", "n", "and", "rav", "e", "at", "clo", "se", "of", "day"]

```

### Brackets and Character Sets with the OR logic operator

`(cat|puma)` is not the same as `[cat|puma]`.

In fact, the `|` logic operator is completely redundant in `[cat|puma]`, which is really `[catpuma]` or any jumbled up sequence of the anagram like `[amuptac]`. Actually, there's a duplicate `a` inside which can be removed: `[muptac]`

What `[]` is, is representation of a single character occupying a space. (This concept of space is important as you will see below with `()`.) Any characters inside the square brackets conveys that this character can be any one of those inside characters. If the regex was `/[ne]/` for instance we would get a match like:

```ruby
"Once upon a time".scan(/[ne]/)
# => ["n", "e", "n", "e"]

# Internally, selection is like:
"O[]c[] upo[] a tim[]"
```

Whereas `(cat|puma)` is really what you expect it to be. It searches for the string literal 'cat' or 'puma'. These brackets `'()'` end up creating a new environment or space for a combination of characters to be evaluated. It's like a regex within a regex. Regexception:

```ruby
"The puma is a big cat".scan(/(cat|puma)/)
# => [["puma"], ["cat"]]

# Internally, selection is like:
"The [___] is a big [__]"
```

What I mean by "space" is that the regex is looking for a particular combination relative to surrouding elements - for example:

```ruby
"The puma is a big cat".scan(/The\s(cat|puma)/)
# => [["puma"]]

# Internally, selection is like:
"[The][\s][puma] is a big cat"
```

Another way to compare `()` with `[]` is to show that the following regex are nearly equivalent:

```ruby
"Hello World".scan(/(e|o|d)/)
# => [["e"], ["o"], ["o"], ["d"]]

"Hello World".scan(/[eod]/)
# => ["e", "o", "o", "d"]
```

### Matching

Although the regex in matchers (`()` brackets) and character sets (`[]` square brackets) are evaluating the same thing, they are being outputted differently. Each match found with a matcher is placed within its own array within an array which may be important for doing other iterations. While in the character set, each match found is just passed into the array as its self.

In order to fix this we add `?:` to the start of the `()` brackets:

```ruby
# The output of this...
"Hello World".scan(/(?:e|o|d)/)
# => ["e", "o", "o", "d"]

# ...is now the same as this:
"Hello World".scan(/[eod]/)
# => ["e", "o", "o", "d"]
```

<br>

Hmph. There's not enough brain juice left in the tank to cover Single Object Evaluation and Modifiers. Another time perhaps.