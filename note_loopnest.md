# Note on LoopNest of LLVM

Charles He. charles.m.hau@gmail.com

May 20, 2021

## Table of Content

- [Note on LoopNest of LLVM](#note-on-loopnest-of-llvm)
  - [Table of Content](#table-of-content)
  - [1 Background: Loop Optimization](#1-background-loop-optimization)
    - [1.1 Loop Representation in LLVM](#11-loop-representation-in-llvm)
      - [Exiting block vs. Exit block](#exiting-block-vs-exit-block)
    - [Rotated Loops and Simplified Form](#rotated-loops-and-simplified-form)
  - [SSA: Single Static Assignment](#ssa-single-static-assignment)

## 1 Background: Loop Optimization

This part are all obtained from:

[reference: 2019 LLVM Developers’ Meeting: “Writing Loop Optimizations in LLVM” by Ettore Tiotto](https://www.youtube.com/watch?v=3pRhvQi7Z10&ab_channel=LLVM)

As mentioned in the video, LLVM Doc Page provides series of terminologies here: [Loop Terminology of LLVM](https://llvm.org/docs/LoopTerminology.html)

### 1.1 Loop Representation in LLVM

What is a loop?

> In LLVM, detection of loops in a control-flow graph is done by LoopInfo. It has three properties:
>
> 1. The induced subgraph (which is the subgraph that contains all the edges from the CFG within the loop) is strongly connected
> 2. All edges from outside the subset into the subset point to the same node, called the header.
> 3. The loop is the maximum subset with these properties.
> [Loop Terminology of LLVM](https://llvm.org/docs/LoopTerminology.html)

#### Exiting block vs. Exit block

Since we know what loop is... then

> An **entering block** (or **loop predecessor**) is a **non-loop node** that has an edge into the loop (necessarily the header). If there is only one entering block entering block, and its only edge is to the header, it is also called the loop’s **preheader**. The preheader dominates the loop without itself being part of the loop.
> A **latch** is a loop node that has an edge to the header.
> A **backedge** is an edge from a latch to the header.
> An **exiting edge** is an edge from inside the loop to a node outside of the loop. The source of such an edge is called an exiting block, its target is an **exit block**.

Easy, right.

Two loops are either disjoint or contains each other.

Loops can be reduced; or not reducible at all.

### Rotated Loops and Simplified Form

Those two concepts are essential to loop optimization.

A **Rotated Loop** converts a for-loop to do-while loop: normally it will reduce CFG to a more canonical form.

Sometimes if the exit condition for a for-loop is not predicable, this behavior kindly lose its ground.

A simplified form is a form contains a pre-header, a existing block, and a latch.

## SSA: Single Static Assignment

It is another simplified form.