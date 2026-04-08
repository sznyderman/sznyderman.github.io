---
layout: post
title:  "AI Will Make Your AI Better"
date:   2026-04-08
categories: AI Coding
---

We're all building AI customizations and tooling. You might build an agent, skill, or reference
document that enables AI do what you need. So how do you make that
agent better? Ask AI to do it.

At first glance, this feels wrong.  Imagine
if someone asks you a confusing question and when they are dissatisfied
with your answer they tell you to ask their question better. If doesn't
make sense. However, most of our interactions with AI are not single call
and response, they're conversations.

After you work through the problem in the chat session, ask
for assistance improving the prompt. The AI isn't improving
the prompt in a vaccuum. It's summarizing the direction, solutions,
and hints that you provided throughout the session.

Here's a real example. I built an agent to assist me with solving
on-call trouble tickets. The agent correctly identified which
logs it needed to analyze and told me to collect them.
In response, I asked the agent to give me a hyperlink to the logs, which
it did. I clicked on the hyperlink, logged in, and copied the error back to the
agent.

At the end of the session, I asked how the agent could be improved. Two
of the suggestion were for the agent to always provide links to logs and for the
agent to have permission to access the logs directly. Both sounded like good ideas,
but I was hesistant to immediately provide permission to
production accounts because of the blast radius of something going wrong.
So I approved the change to always give links, disallowed granting
permission to production accounts, and made a mental note to investigate
restricted access to production accounts in the future.

Improving the way the agent described logs sounds obvious. It's something
I should have realized on my own and implemented without asking the agent.
But remember, I was trying to fix a problem in a
production system. Accessing that log was a papercut that was quickly
forgotten. Since the agent had access to the whole session, it could
learn from the entire set of interactions and propose improvements. 

You can take this strategy beyond setting up agents and extend it
to documentation and runbooks. Whenever you solve a problem with AI that
takes some effort, ask AI to improve the documentation so that next time
it can immediately find the answer without reinvestigating. This is a
small step that makes incremental improvements easy. And those increments
add up.

Don't forget to give yourself credit. The AI isn't figuring
out how to improve prompts or documentation on its own. It is learning
from *you*. The AI is looking back at the conversation that it had with
*you*. The insights, hints, and instructions that *you* gave to the agent
are being incorporated into your agent or docs.

This is a quick and easy step that adds a couple of extra
minutes to your day. You'll earn that time back plus interest tomorrow.
Improving your agents doesn't have to be a scary or stressful. Just
remind AI to learn from you and leverage the lessons later.
