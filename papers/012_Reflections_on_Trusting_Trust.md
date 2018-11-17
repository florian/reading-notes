# Reflections on Trusting Trust [[pdf]](https://www.cs.colorado.edu/~jrblack/class/csci6268/s14/p761-thompson.pdf) [[my blog post]](https://florian.github.io/quines/)

*Thompson, Ken. "Reflections on trusting trust." Communications of the ACM 27.8 (1984): 761-763.*

Quines are programs that when executed output their own source code.
Implementing them is a lot more tricky than one would think.
The fundamental idea is to create a program that produces a self-producing program.
This has implications on computer security.
If a compiler is compiled using its own language, one could alter that compiler to include a Trojan horse.
Even though the corresponding code is removed, its logic still lives inside the compiler and is reproduced every time it is compiled.
This makes it impossible to trust any code that you did not write yourself.

## Introduction

* > I suspect that Daniel Bobrow would be here instead of me if he could not afford a PDP-10 and had to "settle" for a PDP-11
* > That brings me to Dennis Ritchie. Our collaboration has been a thing of beauty. In the ten years that we have worked together, I can recall only one case of miscoordination of work. On that occasion, I discovered that we both had written the same 20-line assembly language program
* At the point of the speech, Thompson had not worked on Unix for some years. So instead, this paper / speech presents the cutest program Ken Thompson ever wrote

## Stage 1

* The goal is to write a program that when executed produces a copy of its own source code
* > Actually, FORTRAN was the language of choice for the same reason that three-legged races are popular
* The idea to solve this is to write a program that produces a self-reproducing program
* Two important properties
    1. The program can easily be written by another program
    2. The program contains excess baggage, e.g. comments

## Stage 2

* When compilers are written in their own language, this yields a chicken and egg problem
* (*Note: It looks like the labels for Figures 2.1 and 2.2 are the wrong wary around*)
* Assume we wanted to add a new character \v to C. Similar to checking for \n, we could check for \v and then return the corresponding escape code
* Once this is implemented and the compiler is recompiled, it knows how to deal with \v. Thus, the line where we check for \v and return the escape code can be altered to return whatever the compiler thinks \v corresponds to
* After the first compilation, the compiler has *learned* what escape code \v belongs to. From now on, the knowledge does not need to be explicitly stored in the compiler code anymore. It is stored in the compiler binary and passed on during every recompilation

## Stage 3

* Assume we wanted to add a backdoor (a *Trojan horse*) using the C compiler. We could alter the compiler to check if `login` is getting compiled, and if so add the backdoor
* That change would be noticed by someone and removed
* However, we could also add a second such mechanism that checks if the C compiler is getting compiled. If so, both Trojan horses are added to the source code
* Again, this requires a *learning phase*. First, the Trojan horses need to be added and the C compiler needs to be recompiled. Then, the system compiler needs to be replaced by the new compiler, after which the source code can be reverted back
* The Trojan horses now live inside the compiler, without anyone being able to see it in the source code

## Moral

* Moral: It is impossible to trust code that you did not fully create yourself
* Instead of the C compiler, an assembler, loader or hardware microcode could be used, too
