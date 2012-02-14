---
layout: page
title: "Project Z"
comments: true
sharing: true
footer: true
---
{% img center /projects/projectz/hero.png There is a small mailbox here. %}

**Project Z** is a Java-based implementation of [Infocom][]'s Z-Machine (specifically, version 3).
It implements – more or less – version 1.0 of the [Z-Machine Standard][], as well as the [Quetzal][]
save format.

The source code for Project Z can be found [on GitHub](http://github.com/mattpf/projectz). It also
depends on an additional project, **[java-iff][]**, which I wrote because I found no simple
implementations of the IFF format for Java. **java-iff**'s API (though not code) is based on 
Python's [chunk][] module, with adjustments to allow for Java's strict typing.

{% img right /projects/projectz/cli.png An alternative UI %}

The actual Z-Machine implementation (in the `zmachine` package) is split from the user interface
(in the `projectz` package), allowing for alternative user interfaces, as shown here.

Why write a Z-Machine? I was intrigued by the binary mess that was ZORK long ago, and one day decided
to figure out what it all meant.

I made a presentation on it, too; [here are the slides](/projects/projectz/slides/). It also discusses
the virtues of assembly over machine code, in case you were curious.

pyzasm
------

But wait, there's more! While working on this Project Z, I also wanted to provide input to the machine!
Since string and instruction coding by hand is tedious, I wrote **[pyzasm][]**, a simple assembler for
the Z-Machine. **pyzasm** is ridiculously trivial, and doesn't support several vital parts of the
Z-Machine (such as the dictionary); however, this:

    print : Hello world

is arguably better than this:

    0300 0001 FFFF 0010 0000 0000 0036 FFFF B211 AA46 3403 945E 2994 A5BB BA

It also allows for somewhat more complex programs, such as a recursive fibonacci program:

    print : Please enter n to calculate fib(n): 
    call @read_int -> $n 
    call @fib $n -> $stack
    print : fib(
    print_num $n
    print : ) = 
    print_num $stack
    new_line
    quit

    METHOD fib n
        jz n rfalse
        je n 1 rtrue
        sub n 1 -> $stack
        call @fib $stack -> $stack
        sub n 2 -> $stack
        call @fib $stack -> $stack
        add $stack $stack -> $stack
        ret_popped

    METHOD atoi table i=1 result char
        .atoi_start loadb table i -> char
        jz char ?atoi_done
        sub char 48 -> $stack
        mul result 10 -> result
        add result $stack -> result
        inc i
        jump ?atoi_start
        .atoi_done ret result
        
    METHOD read_int
        sread *number_table *parse_table
        call @atoi *number_table -> $stack
        ret_popped

    TABLE number_table 6
    TABLE parse_table 10


[Infocom]: http://en.wikipedia.org/wiki/Infocom
[Z-Machine Standard]: http://www.inform-fiction.org/zmachine/standards/z1point0/index.html
[Quetzal]: http://www.inform-fiction.org/zmachine/standards/quetzal/index.html
[java-iff]: https://github.com/mattpf/java-iff
[chunk]: http://docs.python.org/library/chunk
[pyzasm]: https://github.com/mattpf/pyzasm
