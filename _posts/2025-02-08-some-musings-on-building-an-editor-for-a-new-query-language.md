---
title: Some musings on building an editor for a new query language
date: '2025-02-08 16:55:02'
layout: post
---

For almost a year now I've been focused on improving the web-based editor for Elastic's new query language, `ES|QL`.  It's been an interesting and challenging project and I thought I'd record some musings about it.

I think developers can take really good autocomplete and inline validation for granted. I honestly can't think of many cases over the past 5+ years of full-time software development when my IDE made a mistake in identifying issues with my TypeScript or Java code. I've come to trust the red squiglies and the suggestion menus. They're reliable old friends that keep me on the straight and narrow.

But, TypeScript and Java have MASSIVE communities made of thousands of enthusiasts and many wealthy companies. Of course their language tools are going to be spanking awesome.

When you decide to create a completely *new* language and you put lil' ole me to the task of replicating this awesomeness we've all come to expect, I quickly start to learn about the effort it takes to really nail it. Add to that an extra challenge: you decide to release new language features constantly. It becomes a very interesting ride.

The first several months were primarily focused on not flying off the back of the treadmill. 

I inherited an editor that, while being impressive for the small amount of time the previous owner was given to build it from scratch, had many quirks and bugs. Solution architects are posting screenshots of the editor making mistakes in public Slack channels. "It's saying this is wrong but when I submit the query it works." Meanwhile, the Elasticsearch (analytics engine) team just shipped 4 changes to the language. My lead just found a fifth we didn't know about by trawling through their PRs. Did I mention that `ES|QL` is going GA in a month?

You get the idea.

So the first task was to do everything I could to make sure we weren't too embarassed on the GA release. I'll spare you the details but it was a lot of frenzied coding and pinging the analytics engine team.

After the release, the release got extended. Yes, you did read that right. So, there was some more work to do. I'm honestly not complaining; it was stressful but exciting.

However, after the release was actually over, we knew we needed to free up bandwidth, particularly around the consistent additions to the language. It was time to build some robots.

There's something I call `ES|QL`'s *language image* (or just "image").<sup>1</sup> The image of consists of the basic boundaries of what is and is not allowed in a query. It also records what is going to happen when a query is sent to the server. Technically speaking, this encompasses 

1. the ANTLR grammar, which supports syntactical validation
2. a set of function and operator definitions, which are metadata about the signatures, return types, etc., of specific functions
3. semantic knowledge about how each command is put together and what it does
4. other random language rules that are really only defined in the depths of the compute engine (automatic casting for example)

As a language tooling creator you can do no better than the accuracy of your langauge image. It's the upper bound on quality.

[1] I kind of stole this from Don Norman's *system image* but I'm using it with a different meaning.
