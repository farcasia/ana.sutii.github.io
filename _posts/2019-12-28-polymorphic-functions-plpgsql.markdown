---
layout: post
title: "Polymorphic functions and polymorphic types in postgreSQL"
date: 2019-12-28
categories:
  - technical 
description: Polymorphic functions and polymorphic types in postgreSQL
image: /assets/img/Polymorphism2.png
image-sm: /assets/img/Polymorphism2.png
---

One thing that caught my eye in postgreSQL 10 is the type system of functions in the SQL procedural language (PL/pgSQL). More specifically, I am talking about <a target="_blank" href="https://www.postgresql.org/docs/current/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC">polymorphic functions and the associated polymorphic types</a>. The type of polymorphism at play in these functions is <a target="_blank" href="https://en.wikipedia.org/wiki/Parametric_polymorphism">parametric polymorphism</a>, with some twists, as you shall see in the rest of the article.

This is not the first thing someone would find attractive about postgreSQL, but I am very much interested in type systems and their peculiarities. And the type system of postgreSQL polymorphic functions is what I would call peculiar; at least at first sight. That’s mostly because of its restrictions. I was trying to find reasons for why these restrictions are in place, but I could not find any details in the source code comments of postgreSQL, or in the early papers written on postgreSQL. If someone can shed some light on the historical reasons behind these restrictions, I would be most interested in knowing them. In the meantime, I can only try to come up with possible reasons myself.

There are five polymorphic types in postgreSQL: anyarray, anyelement, anynonarray, anyenum and anyrange. They are part of a larger category of so called pseudo-types. There is no reason to go into explanations and examples here, as they are explained and exemplified in the <a target="_blank" href="https://www.postgresql.org/docs/current/extend-type-system.html#EXTEND-TYPES-POLYMORPHIC">documentation</a>; plus, some of them are self explanatory.

Some restrictions of polymorphic types that I find intriguing:

1. Polymorphic types can only be used as types for function parameters or function return types. Thus, they cannot be used as types for column data or function variables, for instance. I assume this restriction is in place to simplify the typesystem of PL/pgSOL because, with this restriction in place, the actual types can be identified from the function call types.
2. If you have multiple polymorphic types in the signature of the function, they all need to be the same type at function call time. I assume this is so that we can make restriction number three work.
3. If you have a polymorphic type as a return type, then you need to have at least one parameter of polymorphic type. This is probably because now the type of the return type can be identified from the function call types.

Let us watch polymorphic types at work in the next section. You will get a feeling of the consequences of these three restrictions.

In the first gist, the `sum_elements` function is a simple function, that sums up an array with elements of any type, `arr`, and the given initial value, `init`. Normally, this would be everything you need as input to this function. In PL/pgSQL though, we still need one more input parameter: a variable of the same type as the array elements. That's because of restriction number one, which says that a polymorphic type can only be the type of a function parameter. We cannot define this variable `el` as a function variable, but we need it to iterate over elements of the array in the body of the function. Thus, I need to declare it as a function parameter. This makes this function signature more confusing than it should be. 
<script src="https://gist.github.com/farcasia/7aad8d5601c1a06d2e31fc71277dedba.js"></script>

Of course, this function will only work for arrays whose elements have the `+` operator. Thus, the call in gist number two would work fine, while the call in the third gist would not. Unfortunately, you would only find out at run time that the `text` type does not support a `+` operator.
<script src="https://gist.github.com/farcasia/00baba75a10a41564980e04b3448f763.js"></script>
<script src="https://gist.github.com/farcasia/b4f585b45d0cc6783eb9e7559426e85f.js"></script>

A question you could ask about these polymorphic functions, is what happens if a function with the same name, and more concrete parameter types is present in the database (think of type `int` instead of all polymorphic types)? In postgreSQL 10 at least, this more concrete version of the function will take precedence. Thus, be aware of that, or otherwise you might get unexpected behavior. 

Obviously, `sum_elements` is only a dummy function, not production-ready, since it completely lacks error handling. It helps illustrate polymorphic functions though, and the extra potential baggage they can bring: non-obvious function parameters, obfuscation by more functions with the same name and more concrete parameters, or more complex error handling code. All this being said, you might still not be able to do without them in certain situations.
