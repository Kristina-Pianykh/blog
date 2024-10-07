+++
title = 'Thesis Learnings, Part 1'
date = 2024-05-10T07:07:07+01:00
draft = false
+++

## The power of positive mindset (even if it’s java).[^1]

It will get you a good head start on the most challenging part of solving a difficult problem with a language you ~~hate~~ slightly dislike.
Having a positive mindset helped me push through the initial learning curve (especially the part with functional Java) and set me on the way of thinking to focus instead on the things that I do find enjoyable. For example, I hadn’t dealt so much with threads before and the way Java handles it makes it a less horrifying experience. But this applies to other things I had to find my way around in Java. Suddenly, the bloated world of alien Java modifiers (like `volitile` and `synchronised`) starts slowly to make sense. After building a a few simple TCP servers you feel slightly more confident and optimistic.

Sooner or later (okay, rather sooner) you get the headache brought to you by the Java infamous object-oriented style. Keeping your scepticism at bay is sure not easy when you consistently find yourself in weird corners trying to do things "the idiomatic way". Don't get me wrong, I do believe that learning a language involves more than just learning its syntax (although it's a topic for a separate post). And having come from procedural programming all the way, I still tried to keep an open mind. But when you find yourself wrapping your utils into a nominal class just because there is no other way in the OOP dictatorship you can't help but wonder if you should side with a crowd of Java haters or it's just a simple skill issue (spoiler: most likely the latter).

Having said that (and hear me out!), the inheritance feature fits in perfectly in my use case where a data stream can hold different class objects and I branch off this stream further based on the properties of the class for each object. At the same time, all the (sub)classes share a massive amount of functionality (all objects represent messages) but each message can behave differently based on its class-unique properties and I want to be able to adjust the granularity of their representation based on whether I want to exploit their unique features or lump them all together as “messages”.

All the while, what really took me a while to understand is polymorphism and interfaces. True, the concept is simple enough but I was hit with it all in the source code of the Apache Flink where I had a hard time understanding anonymous classes (note to myself: read up on it) and interface implementation. It is now that I know that anonymous classes are a single-instance implementations of an interface; that an interface is an absolute abstraction and you have to implement ALL of the listed methods as opposed to extending an abstract class. And while I was famuliar with the concepts of polymorphism and inheritance before, I was arguably overwhelmed when presented a bulky streaming library with all the Java ways of handling these OOP concepts.

All complaints aside, this experience definitely challenged me in many ways but most imprtantly in the way I would usually approach a problem in the procedural programming paradigm. Tricking my brain not to be prejudiced and avoid pushing back against what has been widely criticized helped me stay curious and approach problems with stronger motivation than it would be otherwise. It helped me appreciate the strong sides of the Java world in practice and put into perspective the things I would otherwise do out of habit.

[^1]: This post is part of the series on my 6-month ride through a project for my CS graduation thesis.
