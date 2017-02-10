+++
date = "2017-02-07T20:21:11-05:00"
title = "Fast Username Validation"
draft = true

+++

### Knowing what you don't know: probabilistic data structures

Suppose you run a site that allows its customers to create an account with a
user name and password. Your site is successful and soon more customers flock
to site. Every new customer has to pick a user name. Each has to be unique. At
first that is not a problem.

The first group of users can use their first name. But there are 4.8 million
_James_ in the United States and almost as many _Johns_. About 4 million
_Marys_ and 1.6 million _Patricias_. The [20 most common male names]
(http://names.mongabay.com/male_names.htm) account for 32 percent of the
population. Name collisions start to mar the user experience as users are told,
*"Sorry, that user name is already taken"*.

First initial and last name will go a lot further. However, thanks to human
psychology, we again run out of steam sooner than you expect. At my company,
which employs 5000 people, there were three J. Lloyds. Some first names just
sound better with any given last name. As a parent of five I can attest to
thinking a lot about that.

By the time you reach 50,000 users, most attempts at a short user name require
the user to make repeated attempts to find one that is not taken. To improve
the user experience the UI needs two improvements.

1. Fast turnaround when checking if a given name is taken
2. A visual queue that the name being typed is not yet taken

This type of behavior should be done on the browser (in JavaScript). With these
improvements, your customers will be able to choose a name without the
heartbreak of repeated rejections by the server. Unfortunately, you can't just
download a long list of names. Even a
[trie](https://en.wikipedia.org/wiki/Trie) is too big since a trie will
compress common prefixes but the number of nodes is still proportional to the
number of distinct names. The sheer number of names appears to make a client
side solution impossible. Unless of course you are willing to trade off
certainty for size.

Enter a type of data structure called a Bloom Filter which acts as a set. A
Bloom Filter can compress both the letters and the number of words in a
collection. It is a probablistic data structure. You can't always trust the a
true result for set membership is correct, but a simple formula gives
staticstical limits on the number of false positives. Here is how it works.

...

Facebook has over 1.8 billion active users.  This solution will not scale
to Facebook proporations. 