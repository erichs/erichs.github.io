---
layout: post
title: "Bundling binary blobs"
date: 2014-08-19 09:02:53 -0400
comments: true
categories: Shell Hack
---

Every once in a blue moon, the following hack comes in handy, and I have to look it up again. I don't remember where I first saw this technique in the wild, but I remember being blown away when I realized what was going on.

<!-- more -->

## Effect

A grizzled neckbeard hands you a shell script, and tells you to run it, and that it will install the application you've been wanting to try out.

{% blockquote %}
"What?" you say. "Does it download it from the Internet?"

"No. No network required."

"What, do I run this with Java, or something?"

"No, it is self-contained."

"Do I need to type a cryptic command sequence to get this to run?"

Look, please just run the script. It will install your application."
{% endblockquote %}

Then you look at the file, and see that it is several megabytes long. And, sure enough, it installs your shiny new app. But how?

## Preparation

You will need:

 * a `tar`ed and `gzip`ped file containing the source or binary distribution you want to bundle
 * knowledge of which additional commands to run, if any, to compile, install or otherwise deal with the unpacked files
 * an open mind

## Method

You will be creating a [self-extracting executable][wiki]. The fact that the **'executable'** portion is really **interpreted shell script**, is irrelevant. This technique hinges on the ability of the shell script to reference itself, ignore the shell script portion, and feed the remainder to the shell script as binary data:

{% gist 01bb10aa16a80171358d binary.sh %}

I actually don't prefer to use this version above, but the magic is easy to spot on line 3: `tail -n +8 $0`: `$0` is a reference to this, the current script file, so this command dumps line 8 through the end of the file through a pipeline to a `tar` extraction command. A careful count of the source reveals that line 8 is where the binary tar data was appended.

I don't like this version because you have to manually and carefully count the lines of code in your script and change line 3 accordingly. I much prefer the following snippet which is more robust, but perhaps a bit harder to follow:

{% gist 9cb8986fcd2bd4c11217 improved_binary.sh %}

This version uses file redirection on `$0` to put the contents of the script through the whole shell block between curly braces, and uses a `while read` loop to pre-emptively throw away the lines of STDIN that correspond to the script preamble, before passing the remaining, binary data through tar. The `while` loop also cleverly uses the string matching capability of `case` to treat the `exit 0` as a [sentinel value][sentinel]. Thus, `exit 0` serves as both the start-of-data marker, and the end-of-script command. Wild.

[wiki]: http://en.wikipedia.org/wiki/Self-extracting_archive
[sentinel]: http://en.wikipedia.org/wiki/Sentinel_value
