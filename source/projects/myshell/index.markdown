---
layout: page
title: "myshell"
comments: true
sharing: true
footer: true
---
{% img center /projects/myshell/hero.png Hello, world! %}

**myshell** is an almost completely trivial shell for use in POSIX environments,
written as one question in one homework – though I extended it somewhat beyond that.

The shell supports many standard shell features, including:

- Executing commands! `echo 'Hello, world!'`
- Pipes between processes – `echo 'Hello, world!' | sed 's/Hello/Goodbye/'`
- Redirecting output to files – `echo "mattpf.net" > CNAME`
- Redirecting input from files – `apropos < keywords`
- Variable substiution – `echo $PATH`
- Subcommands – `$(tput bold)$(date)`

The source is available [on Github](https://github.com/mattpf/myshell), and is tested
on OS X and Ubuntu 11.04.

(Why the name, you ask? The exercise called for a `myshell >` prompt.)
