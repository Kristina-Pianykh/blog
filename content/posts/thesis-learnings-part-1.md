+++
title = 'Thesis Learnings, Part 1'
date = 2024-05-10T07:07:07+01:00
draft = false
+++

## The power of positive mindset (even if it’s java).[^1]

Curiosity and a positive mindset go a long way. They will give you a good head start on the most challenging part of solving a difficult problem with a language you ~~hate~~ slightly dislike.
Keeping an open mind helped me push through the initial learning curve (especially the part with functional Java) and shape my thinking to focus on the things that I do find enjoyable. For example, I hadn’t dealt so much with threads before and the standard Java library provides extensive functionality to make threads realively comfortable. Suddenly, the bloated world of alien Java modifiers (like `volitile` and `synchronised`) starts slowly to make sense. After building a a few simple TCP servers you feel slightly more confident and optimistic.

Sooner or later (okay, rather sooner) you get the headache from the Java infamous object-oriented style and the associated boilerplate. Keeping your scepticism at bay is sure not easy when you consistently find yourself in weird corners trying to do things "the idiomatic way". Don't get me wrong, I do believe that learning a language involves more than just learning its syntax (although it's a topic for a separate discussion). And having come from procedural programming all the way, I still tried to keep an open mind to learn how to do things the "Java way". But when you find yourself wrapping your utils into a nominal class just because there is no other way in the OOP dictatorship you can't help but wonder if you should side with a crowd of Java haters or it's just a simple skill issue (spoiler: most likely the latter).

I know it's not hipster to give credit to Java these days but I do appreciate its strong typing, the relatively recent functional feature and (oh my god) inheritance (hear me out!). I had the scenario in my project where a source data stream contains data instances of the `message` type and then branches off into further streams based on the properties of the class for each object. At the same time, all the (sub)classes share a massive amount of functionality (all objects represent messages) but each message can behave differently based on its class-unique properties and I want to be able to adjust the granularity of their representation based on whether I want to exploit their unique features or lump them all together as `messages`.

All the while, what really took me a while to understand in practice was polymorphism and interfaces. True, the concept is simple enough and it wasn't the first time I learnt about it but I was hit with it all in the source code of the Apache Flink where I had a hard time understanding anonymous classes and interface implementation. It is now that I know that an anonymous class is a single-instance implementation of an interface; that an interface is an absolute abstraction that requires you to implement ALL of the listed methods as opposed to extending an abstract class. And while I was famuliar with the concepts of polymorphism and inheritance before, I was arguably overwhelmed when presented with a bulky streaming library with all the Java ways of handling these OOP concepts and a ton of added features.

But let's face it, it's easy to complain (and on Twitter I'm there for every minute of it). Tricking my brain to fight off prejudice and avoid pushing back against what has been widely criticized helped me stay curious and approach problems with stronger motivation than it would be otherwise. It helped me appreciate the strong sides of the Java world in practice and put into perspective the things I would otherwise do out of habit.

[^1]: This post is part of the series on my 6-month ride through a project for my CS graduation thesis.
