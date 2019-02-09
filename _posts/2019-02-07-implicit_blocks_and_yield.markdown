---
layout: post
title:      "Implicit Blocks & Yield "
date:       2019-02-07 16:28:17 -0500
permalink:  implicit_blocks_and_yield
---


Implicit blocks are Ruby’s simplest and most common form of a closure (of which it has several). Unlike a plain method, a closure allows a method to a pass chunk of code to be executed one or more times while that method runs, like a drop-in code snippet. This creates code flexibility and allows for a separation of concerns and to avoid duplication.

`yield` is the keyword used inside a method to call this kind of block. `yield` can be called from anywhere within a method, any number of times. This keyword is unique in that it finds the passed block, executes it, and returns the result to the method at the exact line that it was called.  The Implicit block technically ‘lives’ outside of the method body, but all params and return values passed to and from the block are considered to be in `local scope`.

Implicit blocks are called with comma separated parameters, parenthesis are optional here, but generally a best practice to avoid ambiguity. All params passed to a block follow the same rules as with any method call. These params should be ordered in order of appearance in the code itself, but unlike a regular method call,  implicit blocks can be forgiving if the incorrect number of params are passed. Excess parameters will be set to `nil`. If too many params are passed, the excess params will be ignored unless the `*splat` operators are in play.

Yield called with no block defined will raise a `LocalJumpError`. To protect against this 
blocks have a built in predicate method: `block_given?`. Like all predicate methods this returns true/false and can be used for control flow. The `yield` and `block_given?` keywords allow for implicit passing without code accessing the method directly as it’s not stored in a variable unless otherwise defined.

Implicit blocks belong to the Proc class, but are generally preferred to their parent counterpart Procs (one of the other closure types), for performance reasons, as instantiating a new Proc object object incurs a surprisingly heavy performance penalty.

The value of the last line handled by the Implicit block will be returned to the method from which it was called, on whatever line `yield` appears. This result can be stored in a method variable to be used elsewhere within the method, or simply ignored if something like `puts` is being used to print (`puts` should be used with caution, however, as it will always return `nil`).  This varriable assignment can occur simultaneously with the call to the Implicit block for simplicity.

References:
https://reactive.io/tips/2008/12/21/understanding-ruby-blocks-procs-and-lambdas

https://mudge.name/2011/01/26/passing-blocks-in-ruby-without-block.html

https://blog.appsignal.com/2018/09/04/ruby-magic-closures-in-ruby-blocks-procs-and-lambdas.html.
