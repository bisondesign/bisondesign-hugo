+++
slug = "metric-for-simplicity"
title = "A Metric for Simplicity"
date = "2017-03-01"
menu = ""
draft = true
share = true
tags = ["UX","simplicity", "UI"]
comments = true
image = ""

+++

### The value of simplicity

The most complex requirement in software development is simplicity. In this post I 
will describe a technique for measuring the simplicity of a UI design. This can be
used to choose the simplest design from a selection. 

I recently ran a training exercise in problem solving with an audience of 50
top R&D personelle at a large corporation. Technology companies need to
innovate in order to stay ahead of the competition but innovation requires
problem solving, and before we can solve problems we need to find them. For
example, I never knew that I absolutely had to have a fingerprint reader on my
phone until my wife got one. Whoever discovered that we all shared this common
problem was a genius.

The true genious of a fingerprint reader is that it is so simple; even a
toddler can learn to unlock a phone if the parents are crazy enough to
program his fingerprints on it. Simplification (represented by the meme "Less
if More") is part of refinement, the often skipped last step in problem
solving.

### The meaning of simplicity

Simplicity has different manifestations in different domains. In mathematics it
is often referred to as elegance. An elegant solution is clear, concise, and
surprising. My favorite example is Cantor's demonstration that the real numbers
are uncoutably infinite. Here he explains it to David Hilbert:

<img src="/images/Diagonal-Proof.png"/>

If Cantor's proof had been clunky, the result would have been just as
beneficial to mathematicians. A clunky proof woudn't really matter as long as
it was still correct. Still, elegant proofs are a delight to behold, even on
repeated viewing.

Simplicity in computer science matters. The product (e.g. an app or service) is
not an end but a means to an end. In the UI complexity impedes the end user
from doing his task. In code complexity impedes future maintainability. In
software design complexity impedes flexibility. A clunky UI, a clunky design
and a clunky implementation do matter.

Simplicity in software design, a practice that closely resembles elegance in
mathematics, is the subject of a future blog post. Here we will stick with the
user experience and in particular simplicity in the UI.

### Simplicity metric for the UI

A milestone in the field of usability in computer systems was the publication
of "The Psychology of Human-Computer Interaction" by Card, Moran, and Newell
(1983). The authors proposed a model of the "Human processor" citing ten
scientific principles of operation, like [Fitts's law][fitts] and the [Power Law
of Practice][power] based on empirical data.  

{{< figure src="/images/03-HumanProcessor_Card_Moran_Newell_1983.jpg">}}

Our model of human-computer interaction will be much simpler than the image
above which comes from their book, but we will build on some of the same ideas.
My goal is to show how automated analysis can be added to a UI design tool such
as [Balamiq Mockups][balamiq] to help assess the usability of a UI for performing a
specific task at the design stage.

- model of human behavior while interacting with a UI
- accuracy of model can be judged by its predictive strength
- difficult to evaluate in situ as different users have different experience levels
- use of deep learning (like tensor flow) to refine the model
- 

### References 

[1] Card, Stuart K.; Moran, Thomas P.; Newell, Allen (1983). The Psychology of
Human–Computer Interaction. Hillsdale, NJ: L. Erlbaum Associates. ISBN
0898592437.

[fitts]: https://en.wikipedia.org/wiki/Fitts's_law "Fitts's law - Wikipedia"
[power]: https://en.wikipedia.org/wiki/Power_law_of_practice "Power Law of Practice - Wikipedia"
[balamiq]: https://balsamiq.com/
