# Rotten

Rotten is a small self-hosting Lisp, designed as a vehicle for exploring Ken
Thompson's [Reflections on Trusting Trust][rott].

[rott]: http://cm.bell-labs.com/who/ken/trust.html

## How it works, in brief

1. Rotten compiles to a simple abstract machine ("the VM").
2. The VM is implemented in [Racket](http://www.racket-lang.org/).
3. The compiler from Rotten to VM-code is
   [written in Rotten](http://en.wikipedia.org/wiki/Self-hosting).

## Rotten is really small!

    compile.rot     ~  80 LOC       implements compiler
    vm.rkt          ~ 130 LOC       implements VM
    driver.rkt      ~  90 LOC       implements repl & other conveniences
    TOTAL           < 300 LOC

There are other files in the repository but they're mostly unnecessary, except
for `compile.rotc`, which is the compiled version of `compile.rot` and needed
for bootstrapping!

<!-- TODO: Section about the VM being based on the CAM? -->

# Reflections on Trusting Trust

Rotten is named for Ken Thompson's [Reflections on Trusting Trust][rott], which
demonstrates that a suitably malicious compiler can invisibly compromise any
program compiled by it, including (in particular) itself! This makes for a
wickedly difficult-to-detect bug.

Rotten includes a (mildly) malicious compiler called `evil.rot` which notices
when it's compiling a compiler and injects just such a self-propagating virus
into it. The most interesting part of this process is the need to [quine][] the
virus: since the virus needs to self-propagate, it must have access to its own
source code!

The only other symptom of this virus is that an infected compiler will compile
the symbol `'rotten` to the string `"YOUR COMPILER HAS A VIRUS!!1!eleventyone"`.
This is a poor stand-in for the nefarious behavior a *real* implementation of
RoTT could inject into the compiler, but it will have to do for now.

[quine]: http://en.wikipedia.org/wiki/Quine

# Caveat emptor
This project is an exercise in golfing. Therefore, everything in it is horribly,
horribly bad, including but not limited to:

- the language design
- the VM design
- the interpreter implementation
- the VM implementation
- the heuristic `evil.rot` uses to detect when it's compiling a compiler

**Do not** take any of these as an example of how to do it. If you'd like
pointers on slightly more reasonable ways to design and implement a lisp, feel
free to [email me](mailto:daekharel@gmail.com), although I am not an expert.
