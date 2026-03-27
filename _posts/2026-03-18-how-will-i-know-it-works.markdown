---
layout: post
title:  "How will I know it works?"
date:   2026-03-26
categories: TDD AI
---

With 2026 under way, the majority of software conversations that I have involve AI.
I see this online, at work, and catching up with old colleagues. I'm starting to
see Test Driven Development (TDD) crop up more and more as an option for
drive AI coding. These conversations tend to focus on using an agent to do TDD and always
involve some discussion of unit testing. Whether you choose to use TDD or not,
TDD teaches a valuable lesson that helps us get the most out of AI.
Successful TDD is an approach to problem solving
that forces you to ask how you will know if the problem is solved.

I first experimented with TDD more than 10 years ago. The basic idea seemed simple: write
a test that fails, make it pass, then refactor. However, when I sat down in
front of the computer to work on a challenging problem, I didn't know where to start.
I was supposed to write a test, but what was that test supposed to do?
The struggle when you begin TDD is that it's more than just writing
tests first. TDD is more than a software development strategy, it's a problem
solving methodology. Instead of immediately trying to solve the problem, TDD forces you
to step back and ask how you will know when the problem has been solved.
Before TDD, I approached every problem as follows: how do I make
this work? With TDD, I started by asking how I will know that my future solution
works?

This is a fundamentally different question. Forget about writing software
with TDD for a moment. Let's say that you are moving
to a new city and need to find an apartment. What do you do? One approach is
to start looking at apartment listings. That's easy.
Look at apartments until you find one you like. As you tour each apartment,
you'll know if it works, right? Nothing could go wrong. Any potential issue
will jump out at you. You'll never move into a cute apartment where the bedroom
is too small for your bed, right? (In New York this is possible). If you get
it wrong, you can move. No big deal.

Let's try a different approach. How will I know an apartment works? Maybe you
list the number of bedrooms. Easy. How do I know the bedrooms work? The bedroom
needs to fit a King sized bed, a night stand, and a dresser. That's more precise.
How do I know the bedroom fits all of the furniture? That's tougher. I could visit
each apartent and imagine the furniture it it. Although, I don't know how accurately
I can do that in my head. I could cut out cardboard sheets that represent the
size of each furniture piece and arrange the furniture when I tour the apartment.
That's good! But it doesn't scale. I don't want to carry a sheet of cardboard for
every piece of furniture I own. I could calculate a minimum required room size and
measure the room with a tape measure. That could work! The room must be at least 12 feet by 12 feet
and I will use a tape measure when I tour the apartment. Note that this addresses
both what the room must do (be 12 by 12) and how I will test it (use a tape measure).
I arrived at the raw size, which is a specification. I also arrived at a methodology
to test it.

The specification I arrived at was useful and necessary. But it wasn't sufficient on
its own. Sure, I could find an apartment only knowing the 12 by 12 rule, but
it would be inefficient. What if the real estate posting doesn't list the bedroom
size? What if the listed size is wrong? What if I visit the apartment and eye ball it
incorrectly? Identifying the methodology to verify the specification allows
you to move forward efficiently and confidently.

Asking how you know something works, is only one part of TDD. But it's a critical element
and a skill that's challenging to develop. I've lived that challenge and
observed it in other engineers who are learning to test drive. In a Gen AI world,
this is a critical skill. As we drift farther away from typing each
line of code, we need to have confidence that
AI is building something that works. We need to answer "does it work" confidently and 
unambiguously. And we need to ask repeatedly. An apartment won't change size, but your
software does. It will continue to morph as long as it's useful. Before we can answer that something
works, we have to start by asking how we will know it works.

I'm still experimenting with the best way to leverage Gen AI. The tools are constantly
changing and what I do today won't be what I do tomorrow. However, one thing is constant.
I always start by figuring out how to tell if something works.
