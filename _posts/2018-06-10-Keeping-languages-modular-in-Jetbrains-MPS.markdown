---
layout: post
title: "Keeping languages modular in Jetbrains MPS"
date: 2018-06-10
categories:
  - MPS
  - Modularity
description:
image: /assets/img/IMG_6414.JPG
image-sm: /assets/img/IMG_6414.JPG
---
There are situations when an implementation concern gets in the way
of the modular definition of your languages in Jetbrains MPS.

One particular example is the following. Suppose you have a set of languages
that you defined for your domain. Moreover, suppose that now, somewhere in your implementation, you need to
add factory methods that return a concept from any of these languages you created.
What I need, is to add an interface concept for the purpose of these factory methods (the factory methods
will have as a return type this interface concept). This
interface concept is then inherited by all of the concepts that can be created by our
factory methods. What this entails, is adding an interface concept to the list of inherited interfaces
in all these languages. Is this not an implicit way of breaking modularity, I wonder?
Would it not be good for modularity purposes,
to add these interface concepts somewhere externally?

To be continued ..
