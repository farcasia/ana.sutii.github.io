---
layout: post
title: "Polymorphic functions and polymorphic types in postgreSQL"
date: 2019-12-28
categories:
  - technical 
description: Polymorphic functions and polymorphic types in postgreSQL
image: /assets/img/Consistency3.png
image-sm: /assets/img/Consistency3.png
---
One thing that caught my eye in postgreSQL is the type system of functions in the SQL procedural language (PL/pgSQL). More specifically, I am going to look at <a href="https://www.postgresql.org/docs/current/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC">polymorphic functions and the associated polymorphic types</a>. The type of polymorphism at play here is the parametric polymorphism.

This is not the first thing someone would find attractive about postgreSQL, but I am very much interested in type systems and their peculiarities. And the type system of postgreSQL polymorphic functions is what I would call peculiar at first sight. That’s mostly because of its restrictions. I was trying to find reasons for why these restrictions are in place, but I could not find any details in the source code comments of postgreSQL, or in the early papers written on postgreSQL. If someone can shed some light on the historical reasons behind these restrictions, I would be most interested in knowing them. In the meantime, I can only attempt at coming up with possible explanations myself.

There are five polymorphic types anyarray, anyelement, anynonarray, They are part of a larger category of so called pseudo-types.

Some peculiarities of the type-system that I find intriguing:

Polymorphic types can only be used as function arguments or as function return types. Thus, they cannot be used as column data types or as function variables, for instance. I assume this restriction is in place to simplify the typesystem of PL/pgsql. The actual types can be identified at function call time.
If you have a polymorphic type as a return type, then you need to have at least one argument as a polymorphic type. This is also probably so that we can tell the type of the return type at function call time.
If you have multiple polymorphic types in the signature of the function, they all need to be the same type at function call time. I assume this is so that we can make peculiarity number two work.

Let us watch polymorphic types at work in the next section. You will get a feeling of the restrictions that come with these peculiarities.

<script src="https://gist.github.com/farcasia/7aad8d5601c1a06d2e31fc71277dedba"></script>
<script src="https://gist.github.com/farcasia/00baba75a10a41564980e04b3448f763"></script>
<script src="https://gist.github.com/farcasia/b4f585b45d0cc6783eb9e7559426e85f"></script>
