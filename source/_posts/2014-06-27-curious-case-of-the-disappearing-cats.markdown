---
layout: post
title: "Curious Case of the Disappearing Cats"
date: 2014-06-27 11:02:58 -0400
comments: true
categories: Shell
---
As any Unix veteran will tell you, the `cat` utility is much abused. Here's the common scenario:

``` bash Useless Use of Cat
cat myfile | grep mypattern
```

Of course, this kind of abuse isn't limited to the "pipe to grep" variety. Here's the general form:

``` bash It's not good for my... idiom.
cat <file> | <command> arg1 arg2 argN
```

Indeed, flaming other Unix users for the [Useless Use of Cat][1] has long been the 'national sport' of the Unix tribe, since Usenet days.

<!-- more -->

# What's wrong with this pattern?

One thing, certainly. Two things, probably.

## 1. `cat` is meant to concatenate two or more files together
If you are using `cat` with only one filename argument, then by definition you aren't concatenating anything.

So, here's the proper use of `cat`:

``` bash meow
cat blacklion.txt greenlion.txt redlion.txt bluelion.txt yellowlion.txt > VOLTRON.txt
```

## 2. many commands accept STDIN, but prefer a filename argument
This falls into a related anti-pattern of "Useless Use of Pipe". Instead of:

``` bash Useless Use of Pipe
cat internet_memes.txt | grep -i "chuck norris"
```

Consider:

``` bash He doesn't sleep, he waits...
grep -i "chuck norris" internet_memes.txt
```

# Why is this such a common anti-pattern?
I still find myself occasionally falling into this pattern, despite two decades' worth of keyboard time on Unix variants. I think this habit likely persists for two reasons:

## 1. The habit is learned early
Early bad habits are often hard to break. One of the first things a CLI user wants to do is to read a file, and until the use of pager commands becomes second nature, this will likely lead to:

``` bash
cat somefile
```

For small ASCII files, this may even be an appropriate use of `cat`. Usually though, this early habit leads to a second, more subtle, and persistent mental model.

## 2. `cat` is a conceptual pipeline source
When building up a complex filter, often `cat` starts things off:

``` bash
cat myfile | grep pattern | tr 'A-Z' 'a-z' > outfile
```

The thought process behind this command is: start with some text, then filter it thus, and filter it so, then write it to an output file. Simple. In some cases, I even give myself a pass on this if it makes my intention clearer. Usually, though, I try to fix the habit.

# The Fix?
Refactoring. No, really. More on this next time.

# Bonus: Useless use of `wc`
Often I will do the following to get a matching pattern count:

``` bash
cat file1 file2 | grep pattern | wc -l
```

So, first let's clean up the UUoP:

``` bash
grep pattern file1 file2 | wc -l
```

Turns out, `grep` has a `-c` flag, which displays the count of matched lines, so:

``` bash
grep -c pattern file1 file2
```
[1]: http://en.wikipedia.org/wiki/Cat_(Unix)#Useless_use_of_cat
