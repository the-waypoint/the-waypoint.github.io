---
layout: post-show
title: Git Basics - Exploring Git (PART I)
keywords--0: Git Basics - Exploring Git
title--before: (PART I)
keywords--1: 
title--middle: 
keywords--2: 
title--after: 
---

[@KevinMulhern](https://github.com/KevinMulhern) and I created a [Git Basics lesson series](http://www.theodinproject.com/courses/web-development-101#section-git-basics) in order to convince beginner developers how the Git version control system could (and should) be a powerful tool to include in your development workflow.

Around June of 2016 when I started becoming really interested in web development I tried for the first time ever, to manage a Rails app and push the repo successfully to GitHub. At the time of course I had no idea what I was doing with such a huge framework like Rails. I bumbled my way through the MVC architecture - able to throw that catch phrase around but not actually understand it. Exposure though, was key to understanding the Rails app. I eventually finished getting the mini-blogger app running and what was a frustrating experience at the time made understanding Rails more approachable for the future. But on top of this difficulty was my inability to grasp what Git was actually doing and accomplishing.

It was only until Kevin walked me through pushing a local repository to my remote that I realised I had no way of visualising what Git actually was or how it worked. The problem I ran into was nesting my project folder containing git within a parent folder that also contained git. The cursory `git push origin master` command that I was all too familiar with, but never understood, broke. By the end of the walkthrough I had a plethora of git-related commands that I used to fix the problem but once again did not understand:

* `ls -a`
* `rm .git`
* `git init`
* `git remote add origin git@github.com:csrail/blogger.git`
* `git push origin master`

I never wanted to go through that experience again of trying to feel for Git in the dark - it was almost a reverse-claustrophic experience, where it just seemed like Git was so expansive I would never be able to contain that abyss in my head. So after that excursion, I decided to get a more comprehensive understanding of how Git worked: I hit the [books](https://git-scm.com/book/en/v2), [videos](https://git-scm.com/video/what-is-git) and [documentation](https://git-scm.com/doc) on it.

I honestly did not go very deep into studying - the depth at the time was unfathomable. It would definitely be possible to learn more now that I know much more about Git. But because my programming base does not require Git to that extent, learning Git would only be for my edification.

Much of what I had read was put to personal use: the readings gave me a way to navigate through Git faster and more purposefully. There was however, something irking me: rudimentary questions about Git consistently being asked in the [Odin community chat](https://gitter.im/TheOdinProject/theodinproject). This occurred because every project at TOP (the Odin Project) encourages uploading to GitHub as a way to improve familiarity and understanding of a collaborative developer space. However the resources available at TOP were scarce in this matter. This was indeed exactly why I had so much trouble initially!

Yes, I was tired of troubleshooting easy git problems. <s>I have delayed much of my own programming development to help users who probably do not have the time or tenacity to stick through a whole web development course.</s> 

With a lot of background reading, frustration repeating answers to newbies and wanting to make improvements with TOP's curriculum (mostly so I don't have to repeat myself and so that people can be more autonomous with their learning), I was finally able to make a contribution. 

Kevin, having also noticed the frequency of these problems cropping up decided it was time to make a Git lesson. He knew I was training towards becoming a Git jedi so we both collaborated on what is now a very robust and popular Git basics lesson series.

<br>
[PART II: How One Git lesson transformed into Three.]({% post_url 2016-11-16-git-basics-how-one-git-lesson-transformed-into-three-part-ii %})