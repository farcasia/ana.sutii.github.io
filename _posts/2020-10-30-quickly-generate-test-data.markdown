---
layout: post
title: "Quickly generate test data in postgresql"
date: 2020-10-30
categories:
  - technical 
description:  Generation of test data in postgresql
image: /assets/img/Simple.png
image-sm: /assets/img/Simple.png
---
I often find myself in the situation to create some dummy data to test queries or postgresql functions, both for correctness and for performance. At those times, I want a way to quickly create the test data.

Certainly, it depends on the concrete queries or functions you have, but I find two functions very useful when creating strings, arrays, or calling functions repeatedly: <a target="_blank" href="https://www.postgresql.org/docs/current/functions-srf.html">`generate_series`</a> and <a target="_blank" href="https://www.postgresql.org/docs/13/functions-math.html">`random`</a>.

These two functions are extremely simple:
* `random` returns a random double value between 0.0 (including) and 1.0 (excluding);
* `generate_series` returns a series of values (integers, doubles, timestamps) in a range with a given step.

<h3> Use of generate_series and random </h3>

Next, let's see some examples of using `generate_series` and `random`.

1. Generate text of a given number of characters
    
    Using the <a target="_blank" href="https://www.postgresql.org/docs/13/functions-aggregate.html">`string_agg`</a> aggregate function, and the `generate_series` function, we can create a string of size n. 

    ---
    ```
     -- generates string of size n (100), with n digits
     select string_agg((i%10)::text, '') from generate_series(1,100) as t(i);
    ```

    ---

    or

    ---

    ```
    -- generates string of size n (100), with a character repeated n times
    select string_agg('a', '') from generate_series(1,100) as t(i);
    ``` 
    
    ---

    or

    ---
    ```
    --- generates string of size n (100), with n random characters    
    select string_agg(chr(ascii('a') + (random()*25)::integer), '')
    from generate_series(1, 100);
    ```

    ---
    
2. Generate an array of a given size.

    ---
    ```
    select array_agg(i) from generate_series(1,10) as t(i);
    ```

    ---

3. Call a function n times.

    ---
    ```
    select random() from generate_series(1, 10);
    ```
   
    ---


These two functions are only building blocks for what in the end can be pretty complex test data. 
