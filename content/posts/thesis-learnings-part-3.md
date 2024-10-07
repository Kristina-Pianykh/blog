+++
title = 'Thesis Learnings, Part 3'
date = 2024-07-10T07:07:07+01:00
draft = false
+++

## On input validation and error handling[^1]

* Always validate your inputs. And by that I mean ALWAYS. This will spare you hours of running simulations only to find out that something went wrong and changed the behaviour of the entire system

* Confirm your expectation of the system behaviour. Especially in a non-deterministic aka asynchronous context. ESPECIALLY in a distributed setting with a high data transmission ratio. There’s no humanly possible way to prune gigabytes of log files to verify your assumptions.

For this purposes, I found the [bats](https://github.com/bats-core/bats-core) testing tool for bash quite useful. Instead of eyeballing the logs, toss in a few [`ripgrep`](https://github.com/BurntSushi/ripgrep) commands and format the output with `awk`/`tr`. Running a simple bats script saved me a ton of time detecting faulty behaviour early in the development process. Because even the simplest thing like sending duplicated data in a system built with the "exactly-once" message delivery guarantee can quickly mess up your process.

* Crash early. Even when you have your assumptions about the system behaviour verified and tested, poor error visibility and scope can kill a thread while keeping the program running. With a large enough timeout and numerous threads, this quickly becomes a major pain in the ass. That’s why the decisions on proper error handling should be made early in develolment (e.g. lift the error up to the parent thread or handle locally? crash the program early on a failure or continue with a warning log?). When using an object/function always pause for a sec to think what this object can hold/point to in case something goes wrong. What can this function return on a failure? And how do we want to communicate it? And just to top it all up, Java does not enforce error handling in a way that Rust or Go do (hashtag errors as values), a feature my heart desires as much as a strong typing system.

[^1]: This post is part of the series on my 6-month ride through a project for my CS graduation thesis.
