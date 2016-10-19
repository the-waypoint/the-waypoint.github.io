---
layout: post-show
keywords--0: 
title--before:
keywords--1:
title--middle: 
keywords--2:
title--after: 
---

|---
| HTTP method | URL | Action | Purpose
|-|:-|:-:|:-
| GET | /users | index | get all users
| GET | /users/:id | show | get user object labelled :id
| GET | /users/new | new | get form for making new user object
| GET | /users/:id/edit | edit | get form for updating info on user object labelled :id
| POST | /users | create | create a new user object; its :id is the next available row in your database
| PUT | /users/:id | update | update a user object labelled :id with stuff
| DELETE | /users/:id | destroy | destroy the user object labelled :id in the users [database<sup>\[1\]</sup>](#1)
|---
| Second body
| 2 line
|===
| Footer row