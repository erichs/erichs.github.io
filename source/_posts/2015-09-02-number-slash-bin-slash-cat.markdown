---
layout: post
title: "#!/bin/cat"
date: 2015-09-02 13:08:28 -0400
comments: true
categories: shell
---

Stumbled onto this [post by Russ Cox](http://research.swtch.com/zip) discussing
zipfiles that unzip to themselves. At the bottom, Russ shares his favorite
self-reproducing program, which is also a one-line shell script:

```bash
#!/bin/cat
```

That's it! I love the simplicity of this program, which relies entirely on the
["Shebang"][1] line to feed the contents of the script to `/bin/cat` as data on
STDIN. Many programs interpret the leading `#` as a comment, and will ignore
it. Other programs, which do not have a `#` syntax for comments, will
dutifully ignore the first line of input if it starts with one, thus preserving
the intention of the shebang mechanism.

But, our little friend `cat` doesn't care. She blithely does her thing, and
copies STDIN to STDOUT. In this case, reproducing the program that produced the
output to begin with (a
[Quine][2])!

[1]: https://en.wikipedia.org/wiki/Shebang_(Unix)
[2]: https://en.wikipedia.org/wiki/Quine_(computing)
