---
layout: post
title: "Broken Windows and Broken Code"
date: 2014-06-30 13:31:31 -0400
comments: true
categories: Rants
---
In the [previous post][LastTime] I talked about the Useless Use of Cat, and suggested that this is a habit worth breaking.

Really, though--isn't There More Than One Way To Skin A Cat? If `cat` with a single file argument gets the job done, who besides [Randal Schwartz][PerlGuy] cares?

I care, and I think you should to.

<!-- more -->

The [Broken Windows Theory][BrokenWindows] suggests that by continuing to use a broken mental model of the `cat` utility, when you know of a better way, you may be subtly devaluing your surrounding code, and inviting future abuse. Left unchecked, this kind of thinking forms habits that tend to spider outward and, weed-like, slowly choke the life out of a project.

Saying "Who cares?" to fixable issues within your sphere of control and influence is, in effect, choosing laziness.

Laziness can be a [virtue][LarryWallVirtues], but it has a dark side, and must be employed intelligently.

Laziness in the form of putting off much-needed refactoring leads to design spaghetti and unmaintainable code: Fail. <i class="fa fa-thumbs-down"></i> This is self-defeating behavior that saps your morale, your momentum needed to accomplish the next task, and your motivation to continue learning and growing. This path ultimately leads to the place programmers go to die: maintaining some ancient application that everyone lives in fear of changing, hoping they are too valuable to be fired. Don't be a [Wally][DilbertWally].

Laziness employed in service of, say, Test-Driven Design leads to deferring design decisions and coding the simplest thing that could possibly work: Win. <i class="fa fa-thumbs-up"></i> This is wise coding that stems from an experiential belief that you will know more about your problem domain tomorrow than you do today. Implicit in this belief is an internal drive to be constantly learning, and a promise, to yourself at least, that tomorrow you will clean up any messes you unintentionally make today.

Don't devalue your own worth, sense of accomplishment, and ability to grow. Fix that broken window today.

More on fixing your shell commands as you go, next time.

[LastTime]: /blog/2014/06/27/curious-case-of-the-disappearing-cats/ "Curious Case of the Disappearing Cats"
[PerlGuy]: http://www.smallo.ruhr.de/award.html
[BrokenWindows]: http://en.wikipedia.org/wiki/Broken_windows_theory
[LarryWallVirtues]: http://threevirtues.com/
[DilbertWally]: http://en.wikipedia.org/wiki/Wally_(Dilbert)
