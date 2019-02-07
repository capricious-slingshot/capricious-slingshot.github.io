---
layout: post
title:      "Implicit Blocks & Yield"
date:       2019-02-07 16:28:17 -0500
permalink:  implicit_blocks_and_yield
---


Ruby blocks are Ruby’s simplest and most common form of a closure (of which it has several). Unlike a plain method, a closure allows a method to a pass chunk of code to be executed one or more times while that method runs, like a drop-in code snippet. This creates code flexibility and allows for a separation of concerns and to avoid duplication.

Yield is the keyword used inside a method to call this kind of block. The yield keyword is special in that it finds and calls the passed block and executes it at the exact line that yield was called, so you don't have to add the block to the list of arguments. Yield can be called from anywhere within a method. The block, itself ‘lives’ outside of the method body since it was part of the method call, but is still considered in scope of the method. All parameters and return values passed to and from the block are considered local variables.

Implicit blocks are called with comma separated parameters, parenthesis are optional but generally a best practice to avoid ambiguity. All params passed to a block follow the same rules as with any method call. These params should be ordered in order of apprearance in the code, but unlike a regular method call,  implicit blocks can be forgiving if the incorrect number of params are passed. Excess parameters will be set to `nil`. If too many parameters are passed, the excess params will be ignored.

Yield called with no block defined will raise a `LocalJumpError`. To protect against this 
blocks have a built in predicate method: `block_given?`. Like all predicate methods this returns true/false and can be used for control flow. The `yield` and `block_given?` keywords allow for implicit passing without code accessing it directly as it’s not stored in a variable.

Implicit blocks belong to the Proc class, but are generally preferred to their parent counterpart Procs (one of the other closure types), for performance reasons. as instantiating a new Proc object object incurs a surprisingly heavy performance penalty.

The last value handled by the block will be returned to the method from which it was called on, at the same line of execution as `yield`. This result can be stored in a variable, or simply ignored. `yield` can be called as many times as you want inside a method, and each time the block will return the result of its execution from the specific line number  in which `yield` was called. 

References:
https://reactive.io/tips/2008/12/21/understanding-ruby-blocks-procs-and-lambdas
https://mudge.name/2011/01/26/passing-blocks-in-ruby-without-block.html
https://blog.appsignal.com/2018/09/04/ruby-magic-closures-in-ruby-blocks-procs-and-lambdas.htmlre.
