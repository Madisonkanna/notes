---
title: Fetch Decode Execute
draft: false
tags:
---
Notes from [CSPrimer's lecture](https://csprimer.com/watch/fetch-decode-execute/) on this subject.

What is the fetch-decode-execute cycle?

First basic model of how a CPU works: it fetches, decodes and executes in a loop. A simplified model that reasonably describes many early CPUs.

Notes from[[ CSPrimer's lecture]] on this subject.

You have the CPU executing instructions and memory is somewhere else. In order for an instruction to be executed it needs to reside on the CPU. In order to execute an instruction you need to fetch it from memory and put it in a register. This instruction is a sequence of bits. It may say "add contents of register 1 to register 2". All of this needs to be figured out from bits, so there's a decode unit. The decode unit reads these bits and sends signals on wires elsewhere in the CPU, like the ALU for example. Note: the word wire is used loosely here. The CPU is part of an integrated circuit (a chip) so signals are transmitted via metal oxide and silicon semiconductor components, not the kind of insulated copper wire that you may image when you hear the word wire. 

The ALU will receive bits that indicate that this is an ADD. 

The execute step is the execution. It might happen in the ALU, for example. 

The program counter/instruction pointer (x86 calls it the instruction pointer) stores the address of the next thing to fetch, decode and execute. This happens over and over. 

In a simplified way, a 4gigahertz machine takes 4 billion times per second - you can do 4 billion of these fetch, decode execute cycles in 1 second. 

In reality in fetch, we arent’ fetching instructions 1 by 1, that would be way too slow because it takes so long to get to memory, we get a whole bunch of instructions and populate an instruction cache at the level of the CPU.

