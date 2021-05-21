# Note on Proposal of "Utilize LoopNest Pass"

Charles He. charles.m.hau@gmail.com

## Table of Content

- [Note on Proposal of "Utilize LoopNest Pass"](#note-on-proposal-of-utilize-loopnest-pass)
  - [Table of Content](#table-of-content)
  - [1 Background: LoopNest](#1-background-loopnest)
    - [1.1 Loop Representation in LLVM](#11-loop-representation-in-llvm)
      - [Exiting block vs. Exit block](#exiting-block-vs-exit-block)

## 1 Background: LoopNest

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

> An **entering block** (or **loop predecessor**) is a **non-loop node** that has an edge into the loop (necessarily the header). If there is only one entering block entering block, and its only edge is to the header, it is also called the loop’s preheader. The preheader dominates the loop without itself being part of the loop.
> A **latch** is a loop node that has an edge to the header.
> A **backedge** is an edge from a latch to the header.
> An **exiting edge** is an edge from inside the loop to a node outside of the loop. The source of such an edge is called an exiting block, its target is an **exit block**.

Easy, right.

Two loops are either disjoint or contains each other.