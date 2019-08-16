---
layout: post
title: "Pseudo-types in Postgresql"
date: 2019-05-26
categories:
  - Postgresql
description: The what and how of pseudo-types in Postgresql
image: /assets/img/IMG_6414.JPG
image-sm: /assets/img/IMG_6414.JPG
---
The what and how of pseudo-types in Postgresql

Postgresql's type system is much more different than the type system of the
mainstream programming languages, such as Java and C#.
It probably has to do with maintaining the efficiency of the database, I guess.

Pseudo-types are used as in and out parameters of postgresql functions, but
they cannot be used as types for columns, or variable declarations.

Let's see pseudo-types at work. I will show you three examples, 

<script src="https://gist.github.com/farcasia/2695a1f1a8578355be602e25d5241c79.js"></script>

### Postgresql used in code snippets:

### Bibliography


