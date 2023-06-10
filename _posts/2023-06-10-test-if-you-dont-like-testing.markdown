---
layout: post
title:  "If You Like Testing, Don't Write Tests"
date:   2023-06-10
categories: testing development
---

I'm a strong advocate for automated testing. Over the years, I 
spearheaded multiple initiatives to improve test coverage. People often
assume that I really like testing. But the truth is, I would rather
develop new features.

As a software developer, I spend a lot of time staring at an IDE. That
time falls into three main tasks that I enjoy in the following order.

* Developing production code
* Witing tests
* Debugging defects

I know people who love testing. I'm not one of them. After meeting
many developers during my career, I think I'm the norm.
Given the choice, many of us would rather write production code.

That's why I write tests. Nobody submits
a merge request for 100 lines of code that won't compile (on purpose). You need to
convince yourself that the code you wrote really does what it is supposed
to do. You have two choices:

* Manually test it
* Write automated tests

I prefer automation. Once the test is automated, I don't have
to think about it again. Consider how much manual testing is needed otherwise.

1. Manually test in your local environment
2. Manually test in the beta environment (assuming that you have one)
3. Manually test in the production environment
4. Repeat steps 1 to 3 every time you touch the code again for maintenance,
   bugfixing, or feature extension.

Often times, the same tests can be used in multiple environments. An automated
test you used with your local environment saves you from manually verifying
in beta or production. Regardless, you can always reuse the tests at step 4 
to gain confidence that you aren't breaking anything.

All of the manual testing in step 4 is tedious. And frankly, you might
not remember what testing you did the first time. Or maybe someone else tested it
last time and you don't know how they did the verification. Maybe you skip testing.
Maybe your change impacts a feature that you didn't think would be impacted, so you
didn't consider the need to test it again.

Don't worry. If you break something and don't catch it, someone else will. You can
fix it after it's been raised as a defect. Then you can
debug what's broken, fix it, and test again. If that test had been automated, the
defect might have been caught before it shipped.

I don't have the patience to do all of that testing and debugging. I would
rather work on new features.

The bottom line is that developers have to test and debug. There's no way out.
Writing automated tests is a good way to keep it to a minimum. You can use all of your
new free time to write production code. Or grab a coffee. You deserve it.

If you don't like testing, then the path to freedom starts with writing a single test.
