---
layout: post
title: "Consistency in the CAP theorem vs. consistency in ACID transactions"
date: 2019-11-09
categories:
  - technical 
description: On the different meanings of consistency in the CAP theorem and ACID transactions 
image: /assets/img/Consistency3.png
image-sm: /assets/img/Consistency3.png
---
On the path of becoming a better version of myself, I am trying to read at least a technical book a month (or a few chapters, if the book is very theoretical or on a subject that I am not familiar with). This month I am all into the fundamentals of processing and storing data, laid out in the book <a target="_blank" href="https://www.amazon.com/gp/product/1449373321/ref=as_li_tl?ie=UTF8&camp=1789&creative=9325&creativeASIN=1449373321&linkCode=as2&tag=farcasia-20&linkId=c3c5d9a87595e26b578a02bdb6edbc40" rel="nofollow">Designing Data-Intensive Applications: The Big Ideas Behind Reliable, Scalable, and Maintainable Systems</a> <a href="#ref">\*</a> by Martin Kleppmann. This is truly an impressive book in terms of breadth of concepts and tools in the realm of data applications. Admittedly, at points it can be overwhelming, but it is definitely worth sticking to it.

While reading the book, one thing that struck me, that I did not consider before, are the different meanings of the word consistency in the CAP theorem and ACID transactions. In the following two sections, I will go into the meaning of C (standing for consistency) in ACID and CAP, and into the history of how these acronyms came to be. Understanding the history often helps us better understand the concepts.

<h2> C for consistency in ACID transactions </h2>
The first paper where database transactions are defined in terms of ACID properties (without actually mentioning the ACID acronym, and without considering the I in ACID), is the following: <a target="_blank" href="https://www.hpl.hp.com/techreports/tandem/TR-81.3.pdf">The transaction concept: Virtues and Limitations</a>.
Here, consistency is defined as follows "Consistency: the transaction must obey legal protocols." Nowhere is it mentioned what these legal protocols are. Later in the paper, we find out that "The system provides actions which read and transform the values of records and devices. A collection of actions which comprise a consistent transformation of the state may be grouped to form a transaction. Transactions preserve the system consistency constraints - they obey the laws by transforming consistent states into new consistent states." The language used in the citation is somewhat old-fashioned because the paper is from 1981. In the quote, "the system" is a general application, not necessarily a data management application. Moreover, what a consistent state is, depends on the business requirements of the application itself. 

The paper that coined the word ACID is <a target="_blank" href="https://web.stanford.edu/class/cs340v/papers/recovery.pdf">Principles of Transaction-Oriented Database Recovery</a>. Here, "A transaction reaching its normal end (EOT, end of transaction), thereby committing its results, preserves the consistency of the database. In other words, each successful transaction by definition commits only legal results."
Admittedly, neither this, nor the other definitions are very precise. As also suggested by Martin Kleppmann, maintaining consistency is the responsibility of the application developers. They are the ones that need to make sure the data adheres to certain invariants before and after the execution of the transaction. An example of such an invariant could be the following: in a banking application, the amount of money in an account should never go below zero.

<h2> C for consistency in the CAP theorem </h2>
The CAP theorem, also named Brewer's conjecture, was first mentioned in an invited talk at the PODC (Principles of Distributed Computing) conference. The conjecture, stating the impossibility of having both consistency and availability in the presence of network partitions, was published in <a target="_blank" href="https://users.ece.cmu.edu/~adrian/731-sp04/readings/GL-cap.pdf">Brewer's Conjecture and the Feasibility of Consistent, Available, Partition-Tolerant Web Services</a>. Here, consistency is defined as follows "Under this consistency guarantee, there must exist a total order on all operations such that each operation looks as if it were completed at a single instant." The context for this theorem is a distributed datastore, with replicas (so-called instant in the quote) containing copies of the data.

In the CAP theorem, consistency is equivalent to the so-called linearizability property. Linearizability gives the impression that there is just one copy of the database, even in the presence of multiple replicas. Thus, whenever any client reads any data (from whatever replica), that data is the most recently written data. How that is achieved is a completely different story, and can also be read in Martin Kleppmann's book.

<h2> Conclusion </h2>
In this post I have gone through the meaning of consistency in the CAP theorem and ACID transactions, and I have also touched a bit on the history of these acronyms. This exploration is part of an effort to understand the basics of concepts I make use of in my day to day work.

Until next time!

<p id="ref">* As an Amazon Associate I earn from qualifying purchases.</p>
