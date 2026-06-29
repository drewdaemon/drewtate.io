---
title: Building an editor for a language that changes every week
date: "2025-04-07"
layout: post
tags: [tech]
---

For almost a year now I've been improving the web-based editor for Elastic's new query language, `ES|QL`<sup>1</sup>—a project that’s transforming how humans, systems, and AI agents query data in Elasticsearch.

The `ES|QL` editor has been an interesting and challenging project, especially because developers expect really good autocomplete and inline validation.

I honestly can't think of many cases over the past 5+ years of full-time software development when my IDE made a mistake in identifying issues with my code. I've come to trust the red squigglies and the suggestion menus.

So when you decide to create a completely _new_ language and you put lil' ole me to the task of replicating this awesomeness we've all come to expect, I quickly start to learn about the effort it takes to really nail it. Add to that an extra challenge: you decide to release new language features constantly. It becomes a very interesting ride.

This is the story of a frontend developer taking a journey into the wild and wonderful world of language tooling.

## Diving head-first into GA

The first several months were primarily focused on not flying off the back of the treadmill.

I inherited an editor that, while being impressive for the small amount of time the original owner was given to build it from scratch, was far from mature.

Solution architects were posting screenshots of the editor making mistakes in public Slack channels. "It's saying this is wrong but when I submit the query it works." Meanwhile, the Elasticsearch (analytics engine) team was shipping constant changes to the language. We found ourselves trawling through that team's PRs and pinging people to try not to miss things. Did I mention that `ES|QL` was going GA in a month?

You can imagine the chaos.

So the first task was to do everything I could to make sure we weren't too embarrassed on the GA release. I'll spare you the details but it was a lot of frenzied coding and pinging the Elasticsearch team.

After the release, the release got pushed back. Yes, you read that right. So, there was some more work to do. I'm honestly not complaining; it was stressful but exciting.

But, after the release was actually over, we knew we needed to free up bandwidth, particularly around the consistent additions to the language. I needed to build robots—fast.

## The _language image_

There's something I call `ES|QL`'s _language image_ (or just "image").<sup>2</sup> The image consists of the basic boundaries of what is and is not allowed in a query. It also records what is going to happen when a query is sent to the server. This encompasses

1. the ANTLR grammar, which supports syntactical validation
2. a set of function definitions, which are metadata about the signatures, return types, etc., of specific functions
3. semantic knowledge about how each command is put together and what it does
4. other language rules that are really only defined in the depths of the compute engine (automatic casting for example)

These elements form the ground truth for what a valid `ES|QL` query looks like and how it behaves.

As a language tooling creator you can do no better than the accuracy of your language image. It's the upper bound on quality. This is true because a perfect implementation of a language tool will transparently reflect the facts in the image.

But `ES|QL`'s image was changing weekly. I needed to find a way to regularly export the image from the compute engine in a format that could be consumed by the editor.

## Building robots

### The grammar

First, I tackled the ANTLR grammar. I created a CI job to scrape the grammar from the Elasticsearch codebase on a weekly basis and open an editor PR for my review. There were often still changes that had to be made manually in these PRs, but it gave us a starting point and an extra reassurance we wouldn't miss fundamental changes (like a new command, for example).

The ANTLR grammar was low-hanging fruit because it already existed in a format the editor could consume (after a dependency update anyways). Not so with function and operator definitions which were defined as Java classes.

### The functions

Fortunately, an engineer on the analytics engine team came up with a fabulous idea: record every function signature used during the backend unit tests and publish them as JSON metadata. This is brilliant because it doesn't require extra steps on the part of that team—they are writing unit tests anyways. It also ensures the validity of the exported language image—the metadata are tied to actual function execution.

I started scraping these in my CI job. They weren't a perfect match for our existing system. For example, our manually-maintained definitions relied heavily on single signatures with an "any" type whereas the definitions from the backend had many signatures defining specific parameter type combinations. But with some effort I was able to bring the existing system and the new metadata definitions together.

Not only did it save us time and make us less likely to miss new functions, it also gave the editor a better understanding of each function’s specific behavior.

(Initially, my CI job auto-generated validator tests for every function, but we eventually abandoned that. It created hundreds of tests of dubious value.)

### Phew! 😅

After we completed these two moves toward automation, we started to feel some breathing room.

But what about the 3rd and 4th items in the language image?

Well, they are very difficult to automate. Commands are completely different from one another, not unified by a predictable structure. Other rules such as automatic casting are not represented in the compute engine in a way that can be exported. So, we fell back on humans. We set up Github automation to ping us whenever an Elasticsearch engineer adds a special `ES|QL-ui` label to their PR. That gives us a heads-up when we're going to need to make a change manually.

## Improving our UX

I wrote earlier that the language image is the _upper bound_ on the quality of language tools. We had effectively raised this bound and ensured it would stay high, but these investments were largely behind the scenes with little direct impact on the UX of the editor. It was time to close the gap between our new theoretical best-performing editor and where we were at the time: buggy and incomplete.

We pulled together a “hit list” of everything that made the editor feel buggy or incomplete. Some were small polish items; others were fundamental bugs in autocomplete logic. I didn't use a scientific process, just my judgement based on my own long experience as a language tooling user.

I took on the gnarlier issues, asking another engineer to focus on enhancements.

One by one, the tasks were being completed. I was deep in VSCode's codebase, and all over Monaco's documentation. It was a challenging and exciting time.

The moment it got real? I moved the list from a quiet Google doc to a public Github issue<sup>3</sup>, increasing visibility of our project. We finished up with a well-received presentation to project leadership 💪.

We still have bugs from time to time, but they tend to be small and obscure. We've gotten many compliments on the strength of our editor experience, especially compared to competitors.

## Improving our autocomplete architecture

As you start to "live" in a new codebase you often start to notice inefficiencies—little things that bother or even major things that slow you down significantly. The autocomplete engine was no exception and by this point I had had plenty of time to form an idea of what needed to change.

The original architecture of the autocomplete engine used a declarative interface with a predefined vocabulary of properties to represent each command's syntax. This was then matched up with a query's AST to generate suggestions.

I assume the hope was that the structure of each command would look much the same, like functions do. The problem is that this is not how `ES|QL` has played out. In the grand tradition of query languages, the commands have started looking much different from one another.

We started to see issues with using a declarative interface and these grew with every new command.

- **Code complexity**—the autocomplete code became large, complicated, and difficult to follow. It wasn’t clear which parts of the logic applied to which commands.
- **Lack of orthogonality**—a change in the behavior in one area of the language often had side-effects in other parts of the language.

New syntax and behaviors led our “generic” code to need more and more command-specific branches, and our command definitions to need more and more “generic” settings (that really only applied to a single command).

We needed a far more flexible system, one that would give each command total control over generating suggestions within its own syntax.

🥁🥁🥁 But! I'm not going to spoil what I've done to fix this... because that is coming in another post 😃. Stay tuned!

## Conclusion (am I still a frontend developer?)

That’s the (relatively clean) story of the past year(-ish). In reality, it was a bit more complicated—but this captures the major arcs.

I still run all this code in the browser—but in practice, I’ve been a language tooling developer. It's not too far away: I started my coding journey in C++ so it isn't like web dev is all I've known. I studied programming language theory in my college days, so a lot of that has come back to assist me.

But the key abilities I've gained from my latest years in frontend work—my sense for cognitive psychology, my understanding of how to build a coherent product, and my knowledge of the browser's execution environment— still come in clutch.

The editor exists at the intersection of these disciplines.

Here’s to the future of `ES|QL`. 🥂

[1] [`ES|QL` docs](https://www.elastic.co/guide/en/elasticsearch/reference/current/esql.html)

[2] I kind of stole this from Don Norman's _system image_ but I'm using it with a different meaning.

[3] [https://github.com/elastic/kibana/issues/176033](https://github.com/elastic/kibana/issues/176033)
