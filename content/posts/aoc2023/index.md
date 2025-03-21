+++
title = 'Advent of Code 2023 in Java'
date = 2024-11-07T07:07:07+01:00
draft = false
+++

<!-- ## Advent of Code 2023 in Java -->

Advent of Code is famous in the developer community around the world for providing a perfect setting for learning a new langugage. After all, we all know that no amount of books and documentation reading can do you a better service than the good ol' coding practice (altough RTFM still holds). As a control freak, I like prepping before a challenge. And by that I mean, I ***LOVE*** prepping. And I go hard on it. I'm aware that it's my academic past speaking in the foolish attempt to control uncontrollable but I often can't help and keeping myself on track to solve a problem without side-tracking to investigate all potential rabbit holes along the way can be a challenge in itself. That's why before embarking on a journey with learning Golang by solving AoC 2024 this year, I decided to have a look at the puzzles for last year to know what's coming. And I did it in Java (yes yes don't hate) to build on the flow following from my thesis project[^1].

I came as far as day 10 which crushed me like a bug. After spending 2 days on it I was too frustrated to continue and what remained afterwards was rather cautious anticipation of this year's AoC. I've learnt quite a bit about some features of modern Java (v21). Here's a rather inconsistent log of my impressions along the way:

## Day 2: I love functional Java

Ngl, retrospectively, streaming collections and map/reduce operations are something I now miss in Golang.

## Day 3: dancing around array indices

Tough one. Small input size but I still manage to compute the result in one scan. Part 2: keeping a sliding window of 3 lines with some edge cases for the beginning and the end of the input.

## Day 4: it's all about logging

Ya'll know what I'm talking about about when I mention Java logging. Learning how to use the built-in logger for easier switching between a wall of debug logs and the relevant info-level statements. The decision to avoid using 3rd party logging libs is very intentional: it's ease of setup and use over performance. Rawdogging functional patterns all the way through.

## Day 5: multi-threading and functional interfaces

Large number space. Accumulating computation results leads to the heap memory error. Resorted to a minor optimisation to make comparisons in the end of each iteration to keep only the intermediate result instead of computing the final value given a collection of intermediate values.

Multi-threaded solution results into the reduction of execution time: **40m** --> **1m**.

Learnt about Java functional interfaces (aka interfaces with one abstract method) and most importantly lambda expressions to work easily with functional interfaces. A big bonus point: a lambda has access to the environment of the outer scope! Which makes it much more concise to quickly implement an interface inline instead of defining a separate class and passing tons of params around.

## Day 7: Java enums

Curious sorting problem that I chose enums to solve with. Interesting observation: the result of comparing enum values is determined by the order of their definition.

Practice using lambdas for implementing a functional interface on the example of a custom [Comparator](https://docs.oracle.com/en/java/javase/21/docs/api/java.base/java/util/Comparator.html) for defining custom sorting rules for an array.

Super hard part 2. Insight: sometimes trying to be smart can bite you in the ass. Rawdogging boomer loops instead of fancy looking long chains of functional streaming is the way to go.

## Day 8: LCM and hardest shit yet

Boy oh boy if only I knew what was yet to come.

LCM stands for [less common multiplier](https://en.wikipedia.org/wiki/Least_common_multiple). Like I **REALLY** had to think. Brute forcing didn’t work (I had to kill the program after ~10h+ and it still hadn't terminated). Some other solutions led to out of heap memory or stack overflow issues (and by that I do not mean the garbage pile of a [certain website](https://stackoverflow.com/)). The solution lies not in the problem itself but in the input and the data regularities it contains. Only after inspecting the logs could I detect the patterns and cook up a solution (what I later learnt is commonly referred to as ACM) that cracks the problem in under **1s**🚀

## Day 10: The day I gave up

Pipe maze problem. Part 1 was tough but it's part 2 that took me two days before I could safely pack it in a box and move on. It was one of the problems that is increadibly difficult to debug and instead you have to rely on visualization. Here's what I cooked up in my half-baked attempts:

input text -> loop path -> path + canvas -> just canvas

![loop](images/pipe_loop_short.gif)

My big hope is that I can last longer in AoC 2024.

[^1]: https://kristina-pianykh.github.io/blog/posts/thesis-learnings-part-1/
