---
layout: post-show
title: 
keywords--0: Git Basics - How One Git lesson transformed into Three
title--before: (PART II)
keywords--1: 
title--middle: 
keywords--2: 
title--after: 
---

Find PART I [here!]({% post_url 2016-11-12-git-basics-exploring-git-part-i %})

Git, a very successful version control system, already had many tutorials available online. If you read widely and knew how to piece together large floating bits of concepts and information, you would certainly be able to understand what Git was actually doing for you. But for beginners, the overwhelming amount of theory perused and discovered, would never make more returns than on the investment you put in. In short, documentation on Git was far in abundance, but they were not concentrated. Therefore as a newcomer to the programming scene, you required an exceptionally large knowledge base in order to reliably get up and running. On top of this, your understanding of Git concepts would be very fragile. Your competency would be susceptible to discouragement and challenged at the slip of a command line typo: "be one with the command line you degenerate".

In hindsight, it was clear that I, and many others, were in this very position. Kevin and I knew that this was due to the principle of __information overload__. However knowing the name of the principles is completely different to actually understanding and stripping apart its inner-workings. So it was time for review and reflection.

The end outcome for anyone wanting to learn Git was to be able to use basic commands to understand version control. Namely:

* `git add -A`
* `git commit -m "descriptive message"`
* `git push origin master`

This was not the case though. Teaching these three commands required an understanding of a whole lot of other concepts such as initialising git repos, cloning and the differences between local and remote repositories. What previous teachers thought would be elucidating actually created even more confusion. 

At this point there was a lot of user feedback trickling through and there seemed to be a common trend emerging. Users understood how to use `add` and `commit` commands very well but they were getting seriously entangled by `init`, `clone`, `remote add` and `push` commands. Since everyone learning under The Odin Project was being directed to the same Git resources, I knew (or should I say, made a very calculated assumption) that this was a constant nidus where all the problems arised from.

As any good detective would do, I went to the crime scene to look for clues. I painstakingly went through the basic tutorials and read through the documentaiton prescribed. What eventually surfaced from all the snooping around was a trend that all tutorials shared: every tutorial covered all the previously mentioned commands.

"So what? That was obvious". On a superficial layer, this absolutely does not make sense. But on a deeper level you can actually draw important conclusions.

Whenever one explains a concept to another, you have transferred information from one depot to another. Make another transaction and you would have made two deliveries total. Make 'x' transactions and you would have made 'x' deliveries total. The caveat of this entire process is that all of these transactions have been represented equally. This is certainly a case of "everyone is equal but some are more equal than others." You see, commands like `git init` and `git clone`, although doing very different things, will achieve the same outcome for establishing an empty repository from which to start a project. This here is the key. If you create a tutorial that sandwiches both `git init` and `git clone` within the same tutorial, users will not know where the emphasis or weighting will be. By mentioning both of these commands together in the same location, you are leading users to believe that the commands work together in unison.

For obvious reasons they do not. 

But a newbie doesn't know that.

As teachers and instructors, we have the responsibility of ensuring that learners are given power but not so much power that they do not know what they are doing with it. 

Ironically:

<blockquote>
"With great power comes great responsibility."
<br>
<b>- Uncle Ben</b>
</blockquote>

What was really terrifying was how people learning Git with the current resources available would be running `git init` multiple times in multiple folders to set up for just one project. This sort of scenario needed to be addressed in the up and coming lesson.

<br>
Now preparing for a lesson is not a simple task. It was all too easy to conform and introduce the ideas and the concepts like every other tutorial did. But from first principles, if this sort of approach was successful, we really would not be seeing the results we were seeing now - that is, people struggling with simple Git concepts.

My understanding is that, <mark>for a lesson to be truly successful, it must be exceeding in two qualities: context and pragmatism.</mark>

Providing context connects people's senses, paradigms and current knowledge to the scope of a language or program. You see programs and languages have a lot of layers to them. Telling someone to run commands in what is a novel program to users without first establshing secure ground to learn from means you cannot be certain that what is being taught is viewed and understood from the same perspective. There must be some sort of standardisation in the learning environment in place. When a lesson proves itself to have context, then its message can be conveyed effectively because people are able to know "where they are" within the matrix of a program.

If the items discussed within a lesson do complement each other in order to achieve the same goal then you likely have a context that is conflicting and muddled. For example, the end purpose of trying to use `git init` and `git clone` is to link local and remote repositories together. You then should not be giving users the knowledge to use both. It would be completely unnecessary at this stage because users would try to run both in order to "tick the boxes" with what they learned in their tutorials. 

So we eliminated the notion of the potentially dangerous `git init` command entirely. This also meant that there was no need to convey the `git remote add origin URL` command either which is so much more confusing than you would expect it to be. The confusion is very subtle in that one command. Can you tell? We are trying to teach the command `git add` for staging yet in `git remote` you also have the `add` command. Which takes precedence in the learner's mind then? The answer is neither: both would have been perceived equally by beginners.

The quality of pragmatism is relatively straight-forward. Content from the lessons should be put to immediate use by users. Repetition in the right order and environment will lead to memory retention and familiarity. This means less adrenaline and cortisol messing up with frontal cortex ability. Exposure to concepts beyond the scope of learner's ability is warranted - it's how anyone learns new skills. But those extranneous concepts must remain in context with the overall message; if it doesn't what exactly were you trying to teach in the first place?

In fact, context and pragmatism are two sides of the same coin. You cannot separate one from the other. Context provides you with the ability to apply yourself while pragmatism may only be exercised when you are aware of where you are heading towards.

<br>
The way [@KevinMulhern](https://github.com/KevinMulhern) and I solved these Git lessons was to provide:

* A high level overview of version control systems and their history in our [Introduction to Git](http://www.theodinproject.com/courses/web-development-101/lessons/introduction-to-git)
* A lesson dedicated to the [Git Basics](http://www.theodinproject.com/courses/web-development-101/lessons/git-basics) workflow with powerful metaphors written by me :D
* A [practical project](http://www.theodinproject.com/courses/web-development-101/lessons/practicing-git-basics) to tie all the theoretical concepts together. Which Kevin mostly wrote

This all came about through much brainstorming and ideas being sent back and forth between us. When one idea spawned, it was critiqued, accepted or trashed entirely. We then kept polishing what we had: reading over and over the lessons to see if it made sense to beginners. We polished the lessons so much I could vomit if I ever had to read the `git add -A` command again in the next 48 hours after deployment.

What we produced at the end of it all was a three part lesson series teaching Git Basics. It's really popular and robust lesson series. Kevin and I are both super proud of it.

That's it. That's how one git lesson evolved into three.

Oh yea did I mention Kevin and I are going to roll out a Git Branching lesson series soon?