+++
title = 'Thesis Learnings, Part 3'
date = 2024-07-10T07:07:07+01:00
draft = false
+++

## On input validation and error handling[^1]

* Always validate your inputs. And by that I mean ALWAYS. This will spare you hours of running simulations only to find out that something went wrong and changed the behaviour of the entire system

* Confirm your expectation of the system behaviour. Especially in a non-deterministic aka asyncronous context. Especially in a distributed setting with a high data transmission ratio. There’s no humanly possible way to prune gigabytes of log files to verify your assumptions.

For this purposes, I found the [bats](https://github.com/bats-core/bats-core) testing tool for bash quite useful. Instead of eyeballing the logs, toss in a few [`ripgrep`](https://github.com/BurntSushi/ripgrep) commands and format the output with `awk`/`tr`. Running a simple bats script saved me a ton of time detecting faulty behaviour early in the development process. Because even the simplest thing like sending duplicated data in a system built with the "exactly-once" message delivery guarantee can quickly mess up your process.

* Crash early. Even when you have your assumptions about the system behaviour verified and tested, poor error visibility and poorly scoped error locality can kill a thread while keeping the program running. With a large enough timeout, this quickly becomes a major pain in the ass. That’s why early crashing the program is vital. In a system with a lot of launch surfaces you should also think about lifting and handling errors correctly. When using an object/function always pause for a sec to think what this object can hold/point to in case something goes wrong. And how do we what to communicate it?

[^1]: This post is part of the series on my 6-month ride through a project for my CS graduation thesis.
