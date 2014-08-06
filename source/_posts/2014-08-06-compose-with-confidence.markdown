---
layout: post
title: "Compose with Confidence"
date: 2014-08-06 10:30:02 -0400
comments: true
categories: Shell Composure
---

[Last time][LastTime], I noted that the experience of composing shell functions at the command prompt is awkward. Here's what works for me, and why.

<!-- more -->

## REPLs ❤ Pipes

The shell command prompt is great for quick [REPL][repl] feedback: run a command, watch
the output, tweak the command, and repeat until you have the output you desire.
This is a great way to build up complex command sequences. Feedback is
immediate, and the ability to recall and directly edit recent command history
encourages experimentation.

The prompt is no replacement for a full text editor, however, and there are times that you'll start a command at the prompt and [wish you had done so in an editor][edit-and-execute].

While available for short-term recall in terminal history, command sequences entered at the prompt have a short shelf-life. Shell functions defined (albeit painfully) at the prompt, for example, do not persist across shell invocations.

Composing a complex logic construct or shell function at the prompt is, at best, ephemeral and awkward.

## Scripts ❤ Functions

Script _files_ are the natural habitat for functions and larger programmatic constructs: after all, you have a full 2D editor environment in which to draft, save and revise your code. What might be a struggle and a chore to compose at the command prompt becomes easy inside a text editor.

Persisting to the filesystem means that you can bring all the tools for dealing
with files to bear on your script, including version control--critical to
maintaining code of any real complexity.

Script files have downsides, of course. Chief among them that you lose all the feedback from the command prompt--if you are creating a complex command sequence, you're flying blind inside the editor. Also, you've just inherited all the file-related baggage required to get your script to actually run: shebang lines, executable bits, and PATH considerations.

Composing a complex logic construct or shell function in a script file feels natural, but loses the immediacy of the prompt and can sometimes feel too heavyweight: both factors which discourage the proliferation of lots of small shell functions, which as we've seen can vastly improve the readability and maintainability of our command sequences.

## Best of both worlds?

I care about readability in my shell code. If you're reading this, perhaps you do too. I wanted the best of both worlds, prompt ***and*** editor, so I wrote [composure, a set of opinionated shell functions that help you write shell functions][composure].

In a future post, I'll highlight a few of the features and benefits of writing shell functions with composure, and a few unexpected use cases I've found.

[LastTime]: /blog/2014/07/08/servant-of-two-masters/ "Servant of Two Masters"
[edit-and-execute]: http://nuclearsquid.com/writings/edit-long-commands/
[repl]: http://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop
[composure]: https://github.com/erichs/composure
