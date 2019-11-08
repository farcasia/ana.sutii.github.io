---
layout: post
title: "On the different meanings of consistency in the CAP theorem and ACID transactions"
date: 2019-10-03
categories:
  - technical; databases
description: On the different meanings of consistency in the CAP theorem and ACID transactions 
image: /assets/img/Guardrails.png
image-sm: /assets/img/Guardrails.png
---
Part of my continuous struggle to become a better version of myself is reading at least a technical book a month (or a few chapters, if the book is very theoretical or on a subject that I am not familiar with). This month I am all into the fundamentals of processing and storing data, laid out in the book <a target="_blank" href="https://www.amazon.com/gp/product/1449373321/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1449373321&linkCode=as2&tag=farcasia-20&linkId=c3c5d9a87595e26b578a02bdb6edbc40">Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems</a>. This is truly an impressive book. The breadth of subjects is at points overwhelming, but definitely worth sticking to it.

While reading the book, one thing that struck me, and that I did not consider before are the different meanings of the word consistency in the CAP theorem and ACID transactions. In the following two sections I will go into the meaning of C (standing for consistency) in ACID and CAP, and into the history of how these acronyms came to be. Understanding the history often helps us better understand the concepts.

<h2> C for consistency in ACID transactions </h2>
The first paper where database transactions are defined in terms of ACID properties (without actually mentioning the ACID acronym, and without treating the I in ACID), is the following: <a target="_blank" href="https://www.hpl.hp.com/techreports/tandem/TR-81.3.pdf">The transaction concept: Virtues and Limitations</a>
Here, the definition of consistency is "Consistency: the transaction must obey legal protocols." Later in the paper: "The system provides actions which read and transform the values of records and devices. A collection of actions which comprise a consistent transformation of the state may be grouped to form a transaction. Transactions preserve the system consistency constraints - they obey the laws by transforming consistent states into new consistent states."

The paper that coined the word ACID is <a target="_blank" href="https://web.stanford.edu/class/cs340v/papers/recovery.pdf">Principles of Transaction-Oriented Database Recovery</a> 
"A transaction reaching its normal end (EOT, end of transaction), thereby committing its results, preserves the consistency of the database. In other words, each successful transaction by definition commits only legal results."
Admittedly, these definitions are not very precise. As also suggested by Martin Kleppman, maintaining consistency is the responsibility of the application developers. They are the ones that need to make sure the data adheres to certain invariants before and after the execution of the transaction. An example of such an invariant could be the following: the amount of money in an account should never go below zero.

<h2> Consistency as in the C of the CAP theorem </h2>
The CAP theorem, also named Brewer's conjecture, was first mentioned in an invited talk at the PODC (Principles of Distributed Computing) conference. The conjecture, stating the impossibility of having both consistency and availability in the presence of network partitions, was published in <a target="_blank" href="https://users.ece.cmu.edu/~adrian/731-sp04/readings/GL-cap.pdf">Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services</a>. Here, consistency is defined as follows "Under this consistency guarantee, there must exist a total order on all operations such that each operation looks as if it were completed at a single instant." The context for this theorem is a distributed datastore, with replicas (so-called instant in the quote)  containing copies of the data.

In the CAP theorem, consistency is equivalent to the so-called linearizability property. Linearizability gives the impression that there is just one copy of the database, even in the presence of multiple replicas. Thus, whenever any client reads any data, that data is the most recently written data. How that is achieved is a completely different story.

<h2> Conclusion </h2>
Hopefully you now have a better idea of the difference in meaning of consistency between the CAP theorem and ACID transactions. It's a basic thing, but anything fancy must be built on a strong foundation, made of basics.
Keep on digging the basics! Until next time!


