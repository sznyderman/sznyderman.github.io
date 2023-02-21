---
layout: post
title:  "Roadmap Anchoring"
date:   2023-02-20
categories: planning
---

I recently learned about anchoring bias from Daniel Kahneman's *Thinking,Fast and Slow*[^1]. It made me wonder
how anchoring impacts time and budget estimation for software projects.

If you ever worked on a software project that was delivered late, you are normal. The CHAOS Report 2015[^2] found
that only 40% of projects are delivered on time. And if you also want to be within budget and on target, success
drops to 36%. The bigger the project, the more likely it is to be late.

Large projects are complex. They involve many people. And there are myriad reasons why it is difficult to estimate how 
long they will take. I want to focus on one possible reason: anchoring bias.

## Anchoring Bias

The San Francisco Exploratorium asked a group of visitors two questions:

* Is the height of the tallest redwood more or less than 1,200 feet?
* What is your best guess about the height of the tallest redwood?

The average response was 844 feet.

Another group was asked two (almost identical) questions:

* Is the height of the tallest redwood more or less than 180 feet?
* What is your best guess about the height of the tallest redwood?

They guessed 282 feet.

That's a huge difference! 562 feet. That's a 50 story building.

The gap is caused by anchoring bias. Each group had a different anchor in the first question: 1,200 vs 180 feet. 
Most people know that the tallest redwood is less than 1,200 feet. But when asked how tall it really is, their guess
is pulled up toward the 1,200 foot anchor. In fact, the pull of the anchor is measurable. It's called the
anchoring index:

$$\text{anchoring index} = \dfrac{\text{difference between responses}}{\text{difference between anchors}}$$

For the redwood question, the anchoring index is 55%:

$$\dfrac{844 - 282}{1200 - 180} = 0.55 = 55\%$$

The Exploratorium example demonstrates the anchoring effect when the subject is guessing. Maybe anchoring doesn't matter
if the subject is an expert. After all, if you had already known that the world's tallest tree is a 380 foot redwood
called Hyperion[^3], then anchoring wouldn't have impacted you.

Another study looked at expert estimations. Real-estate agents were asked to assess the value of a house that was 
currently on the market. They visited the house and were given information about it. Everything they were told 
was true except for one thing: the asking price. Half of the agents were provided an asking price signifantly above the 
real listing price. Half were given an asking price significantly below the real asking price.

The realtors were experts and shouldn't have been swayed by the provided listing price. They indicated that the listing
price did not factor into their assessments. Nonetheless, the anchoring effect was 41%. That was barely better than when
the same experiment was done with business students with no real-estate experience. They had an anchoring effect of 48%.

## Anchoring and Software Development

Do software development estimates suffer from anchoring? I think they might. In my experience,
estimates are not done in a vacuum. Achors often appear in the context in which we are estimating. Below are
several examples. In each example, we will assume that the anchor encourages an under-estimate.

**Market timeline**

  You work at a startup that is disrupting the horse industry (e.g. sizing horse shoes with your mobile phone, 
  self-mounting saddles, etc). Product management has developed a list of the critical features that they would 
  like to deliver over the next three years. They also created a timeline of when each feature would have the 
  biggest impact.

The timeline from product marketing is the company's ideal roadmap. It sets an anchor for how long product marketing is
hoping the work will take. It has no impact on how long the work will actually take. It is irrelevent for the purpose of
estimation.

Even if we estimate that the ideal roadmap is unachievable, the anchoring effect will still encourage us to 
under-estimate because we are drawn to the anchor.

**A competitor's anouncement**

  Your company is building a flying skateboard. Yesterday, a competitor announced that they are building a floating 
  skateboard that will be available to customers in one year.

Your competitor's announcement is irrelevant. You aren't estimating how long it takes your competitor
to build a floating skateboard. You are estimating how long it takes for you to build a flying one. You don't know which
features are the same or different for a flying versus floating skateboard. You don't know anything about the 
competitor's resources. You don't even know if their estimate is accurate.

Nonetheless, their estimate creates an anchor that will sway your own.

**Customer information**

  Your customer is designing jetpacks. They hired you to design robots for their factory. They are able to receive robots at the factory in six months.

When your customer is ready to receive the product doesn't change how long it takes you to build it. In fact, you don't 
even know if they actually *need* the robots in six months. It shouldn't impact your estimate, but it will anyway.



The problem with anchoring bias is that we are influenced by irrelevant information in the above examples, *even
if we know it is irrelevant*. That's what we saw with the realtors. They knew that the existing asking price didn't
determine the actual value of the home. That's why they said they ignored the asking price when making their assesment.
Nonetheless, the anchor still impacted their assesment.

Even if you know that you are being provided with dates that shouldn't impact your work estimate, they can still
impact your estimate. And they will often cause us to under estimate.

## Estimates Are Ranges

A study asked people to draw a 2 1/2 inch line going up from the bottom of a sheet of paper. Next, they drew a
line from the top of a sheet of paper until they were 2 1/2 inches from the bottom. Most people estimate 2 1/2 inches
to be smaller when they start from the bottom than when they start from the top. 

You start at an anchor (top or bottom of the page) and draw until you think you are in a plausible range
for 2 1/2 inches. Drawing from the bottom, you stop at the short end of that range. Drawing from the top, you stop at 
the long end of the range.

Consider the flying skateboard project. Let's say that the project actually needs 14 to 16 months. We could 
reasonably estimate the project will take 16 months. That's the outside range of the expected time; our stakeholders can
expect to see the project done within 16 months. We could also estimate the project to take 15 months. Our estimate
will hold true half of the time. We have an equal chance of finishing early or late. If we are planning many projects, 
then our total time spent on all projects can be estimated this way.

It does not make sense to set a deadline of 14 months. That is the least likely result. It sets the stakeholder
expectation based on the best-case scenario. Over the course of many projects, we will acheive average results, not
the best-case. So we will frequently disappoint our stakeholders.

Unfortunately, anchoring encourages us to choose 14 months. We start with a 12 month anchor. We know that is not enough
time. We consider longer times until we reach the first reasonable amount of time: 14 months. And that becomes our 
estimate.

## What Now?

I don't *know* that we need to do anything about anchoring bias. I don't have data that measures its effect in
software project estimation. I don't know if the anchoring index is 41% (like for the realtors) or if it's 4.1%. I *do
know* that people who do software project estimation are human. That means we are subject to anchoring bias. I also
know that we tend to under estimate projects. Anchoring bias is worth our consideration.

So what can we do?

Ideally, we would study anchoring bias in software development. We would measure its impact and measure 
mitigation strategies. That's not a reasonable next step for most software developers and project managers, so what 
else can we do? All we can do is be aware of its impact on us as humans and take measures to protect against it. 
Here are my thoughts:

**Be aware** 

You can't avoid anchoring bias without knowing that it is happening.


**Avoid introducing anchors**

You may not be able to control whether you are exposed to anchors, but you can avoid introducing those anchors
to others. If you are the manager for a large project, you probably already know various dates from customers, the
market, or your sales staff. You can't unknow that information during an estimation meeting, but you can still
avoid biasing.

Avoid discussing customer dates immediately before an estimation task. You should also avoid biasing your teammates.
If you had a customer meeting immediately before your team does estimation, wait to share customer dates until after
the team does estimation.

**Think in ranges**

Think about estimates as ranges and not as fixed times. A low anchor encourages you to select the best-case
estimate, not the most likely. Ask yourself what could go wrong. What risks could cause your estimate to fall short. 
Come up with a range of plausible estimates and select the most likely or the longest. Don't just select the shortest
possible timeline.

**Estimate complexity, not time**

Many agile teams do not estimate time. Instead they estimate the total effort or
the complexity of the task in points. This forces you to consider the complexity of the work relative to other projects
that you have worked on before, instead of against dates that are serving as anchors. 

## Final Thoughts

Software development is a human endeavor. Project estimation is a critical and difficult part of the process. Our (many)
human biases are obstacles to quality estimation. Anchoring bias is hardcoded into our brains and may help to explain 
one of the many reasons that our estimates are so frequently off. As we better understand ourselves, we can nudge our
estimates closer to reality.



---
[^1]: Kahneman, Daniel, *Thinking, Fast and Slow*, chapter 11, 2011.
[^2]: The Standish Group, ["CHAOS Report 2015](https://www.standishgroup.com/sample_research_files/CHAOSReport2015-Final.pdf), 2015, retreived 2023-2-19.
[^3]: Guinness World Records, ["Tallest living tree"](https://www.guinnessworldrecords.com/world-records/tallest-tree-living), retreived 2023-2-19.