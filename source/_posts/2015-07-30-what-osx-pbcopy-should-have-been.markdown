---
layout: post
title: "What OSX `pbcopy` Should Have Been"
date: 2015-07-30 16:32:53 -0400
comments: true
categories: Shell Composure
---
I sometimes wish `pbcopy` and `pbpaste` were the same command, and the command
just Did What I Mean.

Although `pbcopy` and `pbpaste` can be used to do more than simply copy text
data into and out of the general system clipboard, in practice, I never use
those features. In fact, I sometimes find it annoying that there wasn't just
one tool provided that sensed if there was data on `STDIN`, and copied it to
the clipboard, and sensed if `STDOUT` was attached to a pipe or redirect, and
pasted from the clipboard. In fact, the more I thought about it, the more I
thought it *should* do both.

<!-- more -->

This utility could probably be written in C or Go, but I couldn't think of any
reasons I'd want native speed for CLI-based clipboard manipulation, so I
papered over the existing utilities with a bit of shell:

```bash
pb() {
  if [[ -p /dev/stdin ]]; then              # STDIN is attached to a pipe
    pbcopy
  fi

  if [[ ! -t 0 && ! -p /dev/stdin ]]; then  # STDIN is attached to a redirect
    pbcopy
  fi

  if [[ -t 1 ]]; then                       # STDOUT is attached to a TTY
    if [[ -t 0 ]]; then                     # STDIN is attached to TTY
      pbpaste
    fi
  fi

  if [[ -p /dev/stdout ]]; then             # STDOUT is attached to a pipe
    pbpaste
  fi

  if [[ ! -t 1 && ! -p /dev/stdout ]]; then # STDOUT is attached to a redirection
    pbpaste
  fi
}
```

And here it is again with some [composure](https://github.com/erichs/composure) metadata:
```bash
pb() {
  author 'Erich Smith'
  about 'clipboard DWIM tool'
  example 'pb                    # paste'
  example 'echo hi | pb          # copy'
  example 'echo hi | pb | cat -  # copy and paste'
  example 'pb </tmp/file         # copy'
  example 'pb >/tmp/file         # paste'
  example 'pb </tmp/f1 >/tmp/f2  # copy and paste'
  group 'shell'

  if [[ -p /dev/stdin ]]; then              # STDIN is attached to a pipe
    pbcopy
  fi

  if [[ ! -t 0 && ! -p /dev/stdin ]]; then  # STDIN is attached to a redirect
    pbcopy
  fi

  if [[ -t 1 ]]; then                       # STDOUT is attached to a TTY
    if [[ -t 0 ]]; then                     # STDIN is attached to TTY
      pbpaste
    fi
  fi

  if [[ -p /dev/stdout ]]; then             # STDOUT is attached to a pipe
    pbpaste
  fi

  if [[ ! -t 1 && ! -p /dev/stdout ]]; then # STDOUT is attached to a redirection
    pbpaste
  fi
}
```

Credit to Dejay Clayton for his [very informative SO post](http://stackoverflow.com/a/30520299) on the topic of `STDIN`, `STDOUT` and `STDERR` detection.


