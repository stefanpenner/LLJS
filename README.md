LLJS
====

LLJS is a typed dialect of JavaScript that offers a
C-like type system with manual memory management. It compiles to JavaScript
and lets you write memory-efficient and GC pause-free code less painfully, in
short, LLJS is the bastard child of JavaScript and C. LLJS is early research
prototype work, so don't expect anything rock solid just yet.  The research
goal here is to explore low-level statically typed features in a high-level
dynamically typed language. Think of it as inline assembly in C, or the
unsafe keyword in C#. It's not pretty, but it gets the job done.

[Try It Online](http://lljs.org)

Usage
=====

For users of node.js, `bin/ljc` is provided.

For users of SpiderMonkey `js` shell, the compiler can be invoked with:

    $ js ljc.js

in the src/ directory.

asm.js
======

This is an experimental version of LLJS that compiles to asm.js. Not everything is working yet, and dynamic allocation isn't implemented yet, but it will be soon. See [this blog post](http://jlongster.com/Compiling-LLJS-to-asm.js,-Now-Available-).

To run the asm.js tests, run `make asmtest`. To run the benchmark, run `make asmbench`. You can see the working examples in the `test/asm` folder;

You might need to define the SPIDERMONKEY_ENGINE and V8_ENGINE environment variables to run the tests (they default to `js` and `node` respectively). You will also need to define SPIDERMONKEY_ENGINE_NOASM to point to a version of SpiderMonkey without asm.js if you want to run the benchmarks.

Memcheck
========

If you would like to compile with support for [memory checking](http://disnetdev.com/blog/2012/07/18/memory-checking-in-low-level-javascript/) (detects
leaks, accesses of unallocated and undefined memory locations, and
double frees) then compile with the -m flag:

    $ bin/ljc -m -o myscript.js myscript.ljs

And add the following code to the end of your program run to report
any memory errors:

    let m = require('memory');
    // for SpiderMonkey do
    // let m = load('memory.js')
    console.log(m.memcheck.report());

The memory checker uses Proxies so if you use node.js you need to
enable it with:

    $ node --harmony-proxies myscript.js

Testing
=======

To run the tests install the [Mocha](http://visionmedia.github.com/mocha/) module then run:

    export NODE_PATH=src/
    mocha --compilers ljs:ljc

from the root LLJS directory.