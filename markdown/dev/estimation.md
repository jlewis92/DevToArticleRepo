---
title: A developer's guide to estimation
published: true
description: How to estimate
tags: 'estimation, tools, management, beginner'
cover_image: ../assets/dev/estimates/estimateCover.jpg
canonical_url: null
id: 1395819
date: '2023-03-17T16:48:15Z'
---

## Background

I should probably start by saying I *really* dislike estimating.  Most of the time it's a bit like licking your finger and sticking it into the air and the everywhere else it's even worse! Between having to (usually) quantify numerous unknowns that can slow you down, you have to try and plan in whether the co-worker helping you out isn't going to (rightly) go on sick leave just before that major dependency comes in. When you finally do give an estimate, to the people you are giving the estimate to, it's not an *estimate*, it's a guarantee and when it doesn't happen for a multitude of reasons, you get a pretty uncomfortable meeting where you have to try and explain why you didn't do what you said you would.  This feeling is so entrenched into software development that there's a pretty common 'joke' about it:

```text
person: Hey we've got this really cool feature we need adding! All you need to do is add a button to the form
developer: 3 weeks.
person: but it's just adding a button?
developer: ah sure, it'll take 4 weeks.
```

However, none of this really matters, because the business needs this information to try and forecast future work, and if this doesn't happen, the company can go out of business.  So as much as I dislike doing estimates, it's still a key skill for any developer to have.

## How to estimate

So we've established that we all need to know how to estimate, but there's a slight problem in that *how does a developer actually estimate*.  I'm sure most of us with a little experience have just thrown a number out and been reasonably accurate in that prediction without really thinking about it.  This is of course, utterly unhelpful when you're trying to learn how to estimate, and you might read articles such as this one from [Time Camp](https://www.timecamp.com/blog/2022/07/the-complete-guide-on-software-development-time-estimation/) which go through a fair amount of the individual techniques you can use to estimate.  However, in my opinion, the real answer to this we use a mixture of techniques that can give you an estimate in under 30 seconds (really 10) - any longer, and you're probably overthinking it, or the task is too large, and you need to break it down.  Having said that, here is how I personally estimate tasks.

Estimation can really be broken in to three parts for me.  Getting an actual estimate, converting how long I think it will take into a realistic estimate and then finally, how to convey that estimate to the business. I've broken this down below.

## Getting the estimate

When I think about how long something is going to take, I'm essentially running through questions like in the below flowchart to try and draw together an accurate estimate:

<!-- markdownlint-disable-next-line -->
{% codepen https://codepen.io/jlewis92/pen/JjaMyxP %}

**NOTE:** The above isn't everything, just representative of the questions you should be asking yourself before you give an estimate.  For example, if the code I'm writing is using an interesting technology, I'd probably slightly reduce the time, just because I'd put extra work into it.

## Converting to a real estimate

There's a few methodologies you can use to do this, but I'm going to discuss 2 of the more common ones.

### The three point estimate

Frankly, I don't use this *that* often any more, but when I was starting out and had no idea what to do, doing a three point estimate helps you frame the problem in an easy to digest way.

**NOTE:** Three point estimation is the *endpoint* of estimation i.e.: once you've gone through the other steps in trying to figure out how long the task will take

While there are [several](https://en.wikipedia.org/wiki/Three-point_estimation) ways to get to a three point estimation, we're most interested in the quickest way of getting there, so we're going to use the simplest.

Essentially, you ask yourself these 3 questions:

1. How long will it take if it's simpler than I expected
2. How long will it take if everything goes wrong
3. How long is it going to take realistically, based on what I currently know

You then divide the 3 estimates by 3 and then dived them by 3.  For example: I get a task to add a button to form and based on what I know, optimistically it will take me an hour to add, realistically it will take 4 hours, and pessimistically it'll take me 2 days.

This gives me the following:

{% katex %}
{1 + 4  + 16 \over 3} = 7
{% endkatex %}

Now as this is close to 8 hours, I'd then just round up to a full 8 hour day as that's close enough.

### Time plus

This is a really simple way of estimating, and in my experience it's the most popular way of doing this activity.  I *really* wouldn't recommend this for a beginner as it's easy to get wrong, but essentially you take how long you think it's going to take, add 20% and then round up to the nearest of 4 hours, 1 day, 1.5 days, 2 days, 3 days, 4 days and 1 week.  Any more and the task should probably be split apart.

i.e.:

{% katex %}
8 \times 1.2 =  9.6
{% endkatex %}

I then take the 9.6 estimate and then round up to 1.5 days.

This is (usually) far less accurate than a three point estimate, but can get you there most of the time.  In my opinion, despite simpler calculation, this is actually more complicated to do as it relies on already having a full understanding of the solution before you can give an accurate estimate and as I said earlier, better not to rely on if you're new to estimating.

## Conveying the estimate

Conveying can really be broken down into 2 parts.  What to do when you're giving an estimate, and what to do while you're working on the task.

### In the meeting

When you're actually in the meeting, and getting asked for an estimate here are some tips:

- **Take a few seconds to consider before answering**
  - Even if you already know what you're going to be asked to do, it always worth spending a few seconds considering the answer in your head as this is the last chance before the estimate is fixed in people's minds.  On multiple occasions has helped me to remember that one little thing I forgot to consider, allowing me to make a more accurate estimate.
- **Be honest**
  - I know that occasionally, you will get pressured to give shorter estimate than you really expect it to take.  If this is happening to you, trust me it's going to be far less awkward to explain now, than when you have to tell the same people pressuring you it's going to take longer than you said
- **Be honest with yourself**
  - If you're throwing out estimates that you *think* may be possible, but have historically taken you a lot longer (as well as constantly having to explain to the project manager why it's taking longer), it's probably a good idea to take a look at you're estimation process.
- **Don't panic**
  - If you're new to the process of giving estimates, and the other people around you know that if you get it wrong it's not such a big deal, provided you're getting closer every time.  We've all been where you, are, and it's not an easy thing to do when you're just getting started
- **Explain you're reasoning**
  - What I mean by this is, is that just give a short sentence showing you understand the task and set expectations around difficulties you may have.  For example, if I know that I've got dependencies on another team, I'd probably say something like "I can get this done by end of day tomorrow, provided I get dependency X in the next couple of hours".

![Estimation](../assets/dev/estimates/scott-graham-5fNmWej4tAA-unsplash.jpg)

### After the estimate

Once you've given your estimate, there are some things you can do to help try and get the task done on time and also mitigate issues if you do get it wrong:

- **Timeboxing**
  - There are a lot of guides on how to this (such as [this one](https://clockify.me/timeboxing) by Clockify), but in a nutshell it's the practice of allocating a set time in your day when you try and complete the task.
- **Communicate**
  - If you've said a task is going to take a week, and every day you've said everything's going well up until the final day when you suddenly say that you're going to need another week, you are pretty much asking for angry questions to be asked.  It's always better to make sure the project manager understands how you're doing and in my mind, is a major reason for the daily stand-up.  If you're team isn't doing a daily stand-up for some reason, then you probably have access to an instant messaging tool, and you can use that instead.
- **Don't worry if you get it wrong**
  - We've *all* been in this position at some point, the best thing to do is to not get too worried over it, and let somebody know that you might need some help and why you're having issues.  What will get you in trouble is not the underestimation, it's when you try to hide it.
