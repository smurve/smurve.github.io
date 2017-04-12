---
layout: post
title:  "Yet another introduction to neural networks (Under construction)"
date:   2017-04-11 11:49:45 +0200
categories: neural networks
---
### Preface



A lot of introductory material has been written and is still being written about neural networks these days, and some
of the available material has reached an unbelievably high quality in both depth and accessibility, which is a rare
combination. First and foremost, I must mention [Michael Nielson's online book](http://neuralnetworksanddeeplearning.com/) 
and [Chris Olah's blog](http://colah.github.io/), full of utterly
beautiful reasoning, and Ian Goodfellow's popular text book ["Deep Learning"](http://www.deeplearningbook.org/) [{% cite Goodfellow-et-al-2016 %}]. 
So, as an analogy to the well-known saying "I'm standing on the shoulders of giants", I can only conclude that 
I am "wandering between the toes of giants". Hence, I will often-times reference mostly Michael's and Chris' blogs over 
and over again in this text. 


#### Why yet another text?

As I was curiously wandering through the internet, I found a lot of beautiful math, beautiful concepts and
thorough, even more beautiful reasoning (I know I mentioned that already), but in the end there was always a grain of
salt. The provided code examples somehow never truly appealed to me. That may be in parts because of personal gusto - admittedly.
But it's definitely not only because of the used language. Python is pretty much ok, I'm getting used to it. It's because - as 
a software engineer - I learned to love composable solutions so much.

Some frameworks like [Keras](http://keras.io) already provide really nice toolkits/DLSs for deep learning exploration
phases. What I was missing on my learning path was math like in Michael's blog that leads directly to software components
eventually looking more like Keras. Having found none that truly appealed to me, I decided to re-phrase Michael's reasoning
in such a way that it leads right into re-usable components written in Scala. Most of the linear algebra will be handled with the breeze
library, which I learned is meant to bring the rich numPy functionality to Scala community.

#### Yet another introductary text

So here it is: Yet another introductory text on deep learning from math to code.

I'm a nuclear physicist astray - somehow I ended up as a software engineer, and I like to look at  
software engineering as a sequence of
 
1. "decompose a big problem"
2. "solve the then solvable pieces"
3. "assemble the resulting solutions to a big solution"
4. "solve the original big problem".

So what I was always looking for - instinctively - was a toy box of small solutions - perhaps single NN layers - 
that could be easily composed to build arbitrarily deep Neural Networks. Please have a look at the following Scala 
code that creates two complex neural networks and compares their performance in 9 lines of code:

{% highlight scala %}

val NN_part1 = INP ° Conv ° Pool ° SoftMax
val NN_part2a = FCL1 ° SIGMOID ° FCL2 ° SIGMOID ° LOGISTIC_OUT
val NN_part2b = FCL2 ° SIGMOID ° LOGISTIC_OUT

val NN = NN_part1 ° NN_part2a
NN.train(train_set, eta, batch_size)
res_a = NN.test(test_set)

val NN = NN_part1 ° NN_part2b
NN.train(train_set, eta, batch_size)
res_b = NN.test(test_set)

{% endhighlight %}

It is immediately obvious to someone familiar to neural network what's happening here. All the nitty-gritty index juggling, 
formal linear algebra and calculus, and non-linearity is hidden behind well-defined and well-known symbols. Rather than 
looking at a flat block of algorithmic code representing the entire network, I can now understand the big picture 
immediately and afterwards dive into only those parts that I'd like to understand a bit more. Furthermore, here I can 
test the bits and pieces independently, gaining confidence for the functionality of almost any composed network. 
Note the word "almost" here, for the risk of numerical instability lures everywhere. Obviously, there's a price with beauty. 
Most often, it's the performance of the bigger solution. You just can't run in high heels. Some mathematical simplification 
or computational optimizations will not be possible because the parts that could be simplified when regarded together reside 
in different components and "don't see" each other. I'll point out the various locations where that happens.

#### What you'll get

In the proceeding of this blog, I will start with the math, show how the mathematical problem can be decomposed into simple 
functions that support back-propagation. Most of the math will look very similar to the math in Michael Nielson's online book. 
I'll deviate from his calculations, though, to allow for a one-to-one mapping into the resulting code. Obviously, although looking a 
bit differently, my calculations are absolutely equivalent. Afterwards, I model the single functions as composable layers, 
according to the presented math. I'll provide thorough and informative tests to verify and demonstrate the fundamental functions. 
In the very end, I'll add model persistence and parallel execution strategies and end up with what I call a 
"neural network toy box" - a name I chose to clearly distinguish my code from production-ready code seen elsewhere, 
for - after all - I'm just a physicist astray.

#### References

{% bibliography --cited %}
