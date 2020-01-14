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

One thing that caught my eye in postgreSQL is the type system of functions in the SQL procedural language (PL/pgSQL). More specifically, I am talking about <a href="https://www.postgresql.org/docs/current/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC">polymorphic functions and the associated polymorphic types</a>. The type of polymorphism at play here is the <a href="https://en.wikipedia.org/wiki/Parametric_polymorphism">parametric polymorphism</a>.

This is not the first thing someone would find attractive about postgreSQL, but I am very much interested in type systems and their peculiarities. And the type system of postgreSQL polymorphic functions is what I would call peculiar, at least at first sight. That’s mostly because of its restrictions. I was trying to find reasons for why these restrictions are in place, but I could not find any details in the source code comments of postgreSQL, or in the early papers written on postgreSQL. If someone can shed some light on the historical reasons behind these restrictions, I would be most interested in knowing them. In the meantime, I can only attempt at coming up with possible reasons myself.

There are five polymorphic types in postgreSQL: anyarray, anyelement, anynonarray, anyenum and anyrange. They are part of a larger category of so called pseudo-types. There is no reason to go into explanations and examples here, as they are explained and exemplified in the <a href="https://www.postgresql.org/docs/current/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC">documentationm</a>.

Some restrictions of the type-system that I find intriguing:

1. Polymorphic types can only be used as function arguments or as function return types. Thus, they cannot be used as column data types or as function variables, for instance. I assume this restriction is in place to simplify the typesystem of PL/pgsql because now, the actual types can be identified at function call time.
2. If you have a polymorphic type as a return type, then you need to have at least one argument of polymorphic type. This is also probably so that we can tell the type of the return type at function call time.
3. If you have multiple polymorphic types in the signature of the function, they all need to be the same type at function call time. I assume this is so that we can make restriction number two work.

Let us watch polymorphic types at work in the next section. You will get a feeling of the extra restrictions that come with these peculiarities.

The `sum_elements` function is a simple function, that sums up an array with elements of any type, `arr`, and the given initial value, `init`. Normally, this would be everything you need as input to this function. In PL/pgsql though, we still need one more input parameter, which is a variable of the same type as the array elements. That's because of restriction number one, which says that a polymorphic type can only be the type of a function argument. We cannot define this variable `el` as a function variable, but we need it to iterate over elements of the array. Thus, we also declare it as a function parameter. 
<script src="https://gist.github.com/farcasia/7aad8d5601c1a06d2e31fc71277dedba.js"></script>

Of course, this function will only work for array whose types are 
<script src="https://gist.github.com/farcasia/00baba75a10a41564980e04b3448f763.js"></script>
<script src="https://gist.github.com/farcasia/b4f585b45d0cc6783eb9e7559426e85f.js"></script>

Obviously, this is only a dummy function, not ready for production, since it completely lacks error handling.

