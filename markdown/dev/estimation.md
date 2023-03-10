---
title: A developer's guide to estimation
published: false
description: How to estimae
tags: 'estimation, tools, management, beginner'
cover_image: null
canonical_url: null
id: 1395819
---

## Background

I should probably start by saying I *really* dislike estimating.  Most of the time it's a bit like licking your finger and sticking it into the air and the rest of the time it's even worse! Between having to (usually) quantify a large number of unknowns that can slow you down, you have to try and plan in whether the coworker helping you out isn't going to (rightly) go on sick leave just before that major dependency comes in. When you finally do give an estimate, to the people you are giving the estimate to, it's not an *estimate*, it's a guarantee and when it doesn't happen for a multitude of reasons, you get a pretty uncomfortable meeting where you have to try and explain why you didn't do what you said you would.  This feeling is so entrenched into software development that there's a pretty common 'joke' about it:

```text
person: Hey we've got this really cool feature we need adding! All you need to do is add a button to the form
developer: 3 weeks.
person: but it's just adding a button?
developer: ah sure, it'll take 4 weeks.
```

However, none of this really matters, because the business needs this information to try and forecast future work, and if this doesn't happen, the company can go out of business.  So as much as I dislike doing estimates, it's still a key skill for any developer to have.

## How to estimate

So we've established that we all need to know how to estimate, but there's a slight problem in that *how does a developer actually estimate*.  I'm sure most of us with a little experience have just thrown a number out and been reasonably accurate in that prediction without really thinking about it.  This is of course, utterly unhelpful when you're trying to learn how to estimate and you might read articles such as this one from [Time Camp](https://www.timecamp.com/blog/2022/07/the-complete-guide-on-software-development-time-estimation/) which go through a fair amount of the individual techniques you can use to estimate.  However, in my opinion, the real answer to this we use a mixture of techniques that can give you an estimate in under 30 seconds (really 10) - any longer and you're probably overthinking it, or the task is too large and you need to break it down.  Having said that, here is how I personally estimate tasks.

## The three point estimate

Frankly, I don't use this *that* often anymore, but when I was starting out and had no idea what to do, doing a three point estimate helps you frame the problem in an easy to digest way.

**NOTE:** Three point estimation is the *endpoint* of estimation i.e.: once you've gone through the other steps in trying to figure out how long the task will take

While there are [several](https://en.wikipedia.org/wiki/Three-point_estimation) ways to get to a three point estimation, we're most interested in the quickest way of getting there so we're going to use the simplest.

Essentially, you ask yourself these 3 questions:

1. How long will it take if it's simpler than I expected
2. How long will it take if everything goes wrong
3. How long is it going to take realistically, based on what I currently know

You then divide the 3 estimates by 3 and then dived them by 3.  For example:  I get a task to add a button to form and based on what I know, optimistically it will take me an hour to add, realistically it will take 4 hours and pessimistically it'll take me 2 days.

This gives me the following:

$${1 + 4  + 16 \over 3} = 7$$
