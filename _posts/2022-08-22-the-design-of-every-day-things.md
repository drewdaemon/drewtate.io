---
layout: post
title: "Synopsis of the first chapter of <i>The Design of Everyday Things</i>"
tags: "reading notes"
---

## The question of responsibility

Are machines responsible to work well with humans, or are humans responsible to learn how to work with machines? Engineering types (of which I am one) tend to emphasize and embrace the logic of what they are building. If a user runs into trouble, we're stunned. They just need to think rationally! Did they even read the manual?

Instead, we need to design everyday machines that are delightful and helpful to _real humans_ who experience the world subjectively and don't have time to take an evening class on how to use their new microwave.

In other words

> It is the duty of machines and those who design them to understand people. It is not our duty to understand the arbitrary, meaningless dictates of machines.

Norman, for one, does not welcome our robot overlords. Fair enough.

## Principles of human-centered design (HCD)

In between illustrative stories and examples, Norman is both introducing us to fundamental principles of good design, and giving us a vocabulary by which to describe and evaluate designed objects.

Well-designed objects exhibit two characteristics:

- Discoverability — the user is able to discover the possible actions of an object.
- Understandibility — the user is able to grok the purpose of the object... they "understand what it all means."

We are introduced to some theory we can use in pursuit of designs that satisfy these requirements.

### Affordances

First is _affordance_. An affordance is a possible action that can be accomplished in collaboration between an agent (a human) and an object. For example, a chair can be sat in. The chair _affords_ this action.

It is easy to focus solely on the affordance of the _object_, but the _agent_ must also be capable of collaborating on the action. Does a fifty-pound bag afford lifting? That depends on the strength of the agent.

### Signifiers

While designers have come to use "affordance" to describe the sensory cues that make affordances discoverable, affordances actually exist independently of how obvious they are made to the user. Thus, the proper term for these sensory cues is the "signifier."

In most cases, the goal of the designer is to maximize discoverability by aligning the signifiers of an object with the affordances thereof. For example, the line under a hyperlink is a signifier designed to make the affordance of being able to click the link discoverable. However, in other cases, signifiers are designed to obscure an affordance when it is not the designer's wish to have it known.

### Mapping

In my apartment, I have three light switches in a row on my wall. They are located in the living area. The one in the middle controls the lighting in the living room area. The one furthest from the dining table controls the light that illuminates the dining area. And the one closest to the dining area does nothing (..._as far as I know_).

I've lived here for three months but, when it's time to eat, I still occasionally reach for the switch closest to the dining area. My light switches are badly _mapped_, leaving me to my trial-and-error.

Mapping means harnessing the power of spatial analogies in design. When done successfully, the designed object is immediately understandable because the user can draw upon an obvious frame of reference.

### Feedback

The idea behind feedback is that the results of an action should be communicated to the user. This is what is happening when an elevator button becomes illuminated after being pressed. The machine is signalling that it is working to accomplish your wishes.

Feedback is most critical when something is going wrong. It is often straightforward to design a product that works well as long as everything goes the way the designer intends. Products that provide great feedback can get the user back on track (or at least be somehow helpful) when things _don't_ go as planned.

However, one has to walk a line. Too much feedback can be extremely distracting and annoying. It can also cause users to miss out on important information since it is drowned out in a roar of verbosity. This is why we have log levels in good software.

Take [this recent Kibana issue](https://github.com/elastic/kibana/issues/67270) for instance. When things go wrong, users are sometimes spammed with a wall of error messages of varying degrees of importance. This is something we need to address.

### The conceptual model

The design of a product should be understandable. A big part of this is that it should accurately communicate useful ideas about the inner working of the machine. It doesn't actually matter how specific or abstract (or even technically accurate) these ideas are in the user's head, as long as they hold in all scenarios the user will face.

Products are often designed with simplicity in mind. However, there is a danger in pursuing simplicity too strenuously. A design that communicates an oversimplified conceptual model can instill inaccurate beliefs in the user's mind that are ultimately unhelpful or confusing. I see this often in complex software. (E.g. Lens has a "top values" function that hides some [serious complexity](https://discuss.elastic.co/t/top3-in-line-graph-and-top-values-in-available-fields-are-different/310382/4).) This is another difficult line to walk.

### The system image

The designer has no direct communication with the user. Instead, a set of physical artifacts which can include documentation as well as the designed object itself are left to do the talking. These artifacts compose _the system image_. The system image communicates affordances, gives feedback, and imparts a conceptual model to the user's mind. Altogether, the system image is what the user will _experience_.

## In conclusion

Norman concludes with a few thoughts around how advances in technology currently outpace advances in design.

Effective design is also made difficult by how much orchestration it takes. The engineer will push for stability, the marketer will push for cutting costs and adding features, the store wants something that will be attractive to its customers. The product must satisfy the requirements of both those who buy it and those who use it, often separate groups of people (think CIO making decisions about technology vendors).

Despite this, he assures us that

> The goal is to produce a great product, one that is successful, and that customers love. It can be done.
