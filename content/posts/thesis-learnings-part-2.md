+++
title = 'Thesis Learnings, Part 2'
date = 2024-06-10T07:07:07+01:00
draft = false
+++

## On abstraction and ugly code[^1]

* **DO NOT ABSTRACT UNLESS ABSOLUTELY NECESSARY!** Yep, I said it. Abstractions are beautiful in theory and Java makes it so tempting to start with an abstraction and work your way down to specifics. Fight the urge to abstract early, you’ll thank me later.

* This goes in hand with the above but I’ll make a separate point out of it. Do not try to be a smart-ass. Clean code, modularisation - out of the window. This is unimportant in the POC stage. Hack it out as fast as possible to get a working solution. I'd go even further than that. Make it your goal to make it as ugly as possible. Avoid early optimisation and getting stuck on splitting your code into functions 4 lines of code each (I'm sorry, Uncle Bob). Raw dawg 50 line-functions all the way, embrace code duplication. Embrace chaos. Only when it starts getting really uncomfortable for you (and by that I don’t mean your perfectionism), only then do some refactoring. Don’t let some abstract principle guide your hand. Make it dirty and functional because you learn so SO many things about the process.

I'll give an example. When I was working on a custom monitoring component for my Flink workload, I was presented with three different implementation ideas. Prettyfying from the start would have wasted my time since I had to hack out each of these approaches before arriving at a working solution.

[^1]: This post is part of the series on my 6-month ride through a project for my CS graduation thesis.
