---
layout: post
title:  "Limit AI Generated Lines of Code"
date:   2025-08-09
categories: AI Coding
---

Using Gen AI to develop software means reviewing code. A lot of code. Gen AI is a powerful
iterative tool. Sometimes the first AI output does the job; sometimes it takes a
few tries. Reading AI output is an important part of the process.

The importance of reviewing AI generated outputs is something that I hear about again and again.
It comes up in meetings and blogs. You'll even find a reference to it in Anthropic's
Terms of Service [^1].

> It is a Customer's responsibility to evaluate...Outputs..., including where
human review is appropriate... Outputs should not be relied upon without independently checking
their accuracy, as they may be false, incomplete, misleading or not reflective of recent events
or information.

As a human, I want to optimize the outputs for human
readability. More specifically, for this human. Me. AI is happy to make large 
code changes. Unfortunately, reviewing large code
blocks creates challenges for human reviewers.

* Reduced reading comprehension
* Missed defects
* Long feedback loops

To avoid these problems, limit the size of AI generated code changes. Let's look 
at the benefits of keeping AI outputs short.

## Improves reading comprehension

Studies show that reading comprehension is better on paper than on screens [^2].
It isn't practical to print every AI output to paper. However, studies that
compare paper and screens give us hints on how to optimize our screen
experiences. Below 500 words, there is little difference in comprehension between
paper and screens. For longer texts, paper wins. Scrolling is one of the problems.
Scrolling may introduce more cognitive demand that reduces comprehension. If the AI
output is too big to fit on your screen, limit the output size to avoid scrolling.
Better reading comprehension means better reviews, better follow up prompts, and
better results.

## Catches more defects

A Smartbear study of
a Cisco Systems programming team made several recommendations for human reviews [^3].
They recommended keeping reviews below 400 lines of code (LOC) because defect detection falls off
dramatically above the 200 to 400 line range. However, it's worth noting that finding
70% to 90% of defects in a 200 to 400 LOC review required 60 to 90 minutes of review.
That's significantly more time than I want to spend reviewing each AI output.

It's tough to maintain focus for 60 minutes. When there are too many line,
I lose concentration and begin to skim the model output. I want to keep code reviews
short enough that I can easily maintain focus. Since I will be iterating through the
day, I prefer to keep the LOC short enough that I can limit each review to a few minutes.

## Shortens iterative feedback loops

It takes time to review the outputs. The longer the output, the longer it takes 
to review and get to the next prompt. Shorter output lengths mean shorter iterations.
Shorter iterations mean you can re-prompt more quickly.

The average reviewer can read 500 LOC per hour [^3]. That's ~7 seconds per line. Using that
approximation, we can convert a target iteration time to LOC. For example, if you want
to spend a max of five minutes reviewing, then you would limit the lines to ~40.

Note that ~7 seconds per line is an average. You may read slower or faster and that's
okay. Pay attention to your feedback loops and make adjustments until you are comfortable.


## Context tradeoff

There's a meaningful cost to short reviews: reduced context. If the 
model output is too small, you won't
have enough context about how the suggested code will fit into the larger project.
Taken to an extreme, imagine reviewing one line
of code at a time and giving a yes or no. It would be impossible to give meaningful
feedback. You would need to approve several lines before knowing if the 
first line needed to be adjusted or not.

## Find Your Sweet Spot

The numbers above are averages for engineering teams. They provide a starting
point. Your needs may vary. For example, I use a larger font size on
my screen than the other members of my team. That doesn't mean my teammates should
be using larger text (they can read it just fine). And it doesn't mean I should
make my text smaller (I need to be able to read without straining).

Personally, I use a 35 line limit because it works for me. Without a line limit,
I find myself prone to accepting insufficiently reviewed code because of
cognitive overload due to scrolling and long iterations. I will eventually
accept AI suggestions without sufficient vetting. In complex projects, this leads
to hard-to-fix defects. At the other extreme, I tried limiting the lines to 20.
I found this reduced context too much. It became hard to understand where the AI
was going and I would end up accepting two or three AI outputs only to have to go
back and edit the first.

After experimenting with multiple configurations between 20 and infinity, I settled
on 35 as a good balance. I have sufficient context and I find the reviews easier and
more effective. I also like the size of the iterative feedback loop. I doubt that I will
use a 35 line limit forever. AI tooling will change, my projects will change, and I
will change. However, the larger concepts driving my decision are unlikely to change.
As a human, I am still going to need to read effectively, identify defects, and iterate.

Tuning model output to you is critical to success. At the end of the day, you are the
developer. AI is an assistant; you are the owner. Limiting line length is one
way to tune AI to make you, the human, more successful.

---
[^1]: Anthropic, [Commercial Terms of Service](https://www.anthropic.com/legal/commercial-terms), retrieved August 3, 2025.

[^2]: ScienceDirect, [The smell of paper or the shine of a screen? Students’ reading comprehension, text processing, and attitudes when reading on paper and screen](https://www.sciencedirect.com/science/article/pii/S0360131524001210), retrieved August 3, 2025.

[^3]: Smartbear, [Best Practices for Code Review](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/), retrieved August 3, 2025.
