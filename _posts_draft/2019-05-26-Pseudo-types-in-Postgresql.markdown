Not valid
---
layout: post
title: "Polymorphic types in Postgresql"
date: 2019-05-26
categories:
  - Postgresql
description: Polymorphic types in Postgresql
image: /assets/img/IMG_6414.JPG
image-sm: /assets/img/IMG_6414.JPG
---
Polymorphic types in Postgresql

Postgresql's type system is different from the type system of the
programming languages that I am used to, Scala, Java, or C.

Polymorphic types are used as in and out parameters of postgresql functions, but
they cannot be used as types for columns, or variable declarations.

At first I was amazed at the way these types worked in postgresql.

Let's see polymorphic types at work. I will show you three examples, 
 
<script src="https://gist.github.com/farcasia/2695a1f1a8578355be602e25d5241c79.js"></script>

### Postgresql used in code snippets:
https://www.postgresql.org/docs/10/extend-type-system.html

### Bibliography


