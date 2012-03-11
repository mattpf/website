---
layout: post
title: "Impossible problems"
date: 2012-03-11 00:48
comments: true
categories: Programming
---
The midterm exam for one of my classes featured three questions, all of which were to
be answered with some relatively simple coding. Aside for one minor flaw: the first
question *was not possible*. Mostly.

Below is a minimal reproduction of the question, with all the irrelevant details (and,
for that matter, the original question) omitted, leaving only the flaw that made it
impossible.
<!-- more -->
* * *

Here is some code:

``` cpp
#include <cstdlib>
#define MAX_ENTRIES 23

class SomeClass {
    public:
        SomeClass();
        int countThing();
        int countAllTheThings();
    private:
        int n;
};

SomeClass::SomeClass() {
    n = rand() % 10;
}

int SomeClass::countThing() {
    return n;
}

int SomeClass::countAllTheThings() {
    // Implement this.
}

int main() {
    SomeClass things[MAX_ENTRIES];

    // Do things with the things.

    return 0;
}
```

Implement `SomeClass::countAllTheThings` such that it calls `countThing` on all the
entries in `things` (declared in `main`). `SomeClass::countAllTheThings` must not
take any parameters. The invocation of `countAllTheThings` will be from the `main`
function.

Any ideas?

* * *

Here is *a* solution; I do not claim it to be a good one. But then, I don't claim
that the problem, as specified, has *any* good solutions.

``` cpp
// This may only be invoked as follows, from the main function.
// Any other invocation is invalid.
//   things[0].countAllTheThings();
int SomeClass::countAllTheThings() {
    int total = 0;
    for(int i = 0; i < MAX_ENTRIES; ++i) {
        total += (this + i)->countThing();
    }
    return total;
}
```

This works because object has an implicit pointer to itself, `this`. Provided we know
the method is called on the first (0th) element of the array, and that the size of the
array is `MAX_ENTRIES`, we can use [pointer arithmetic][] to find references to every
other member of the array. Thus, the solution is *valid*, but neither particularly
elegant nor remotely reusable. Or sane, for that matter.

An alternative, *possibly* neater solution might be to set a reference to `things` on
the `SomeClass` instance itself, then call `countAllTheThings` with a reference to that.
Not much better, though.

[pointer arithmetic]: http://en.wikipedia.org/wiki/Pointer_(computer_programming)#C_and_C.2B.2B
