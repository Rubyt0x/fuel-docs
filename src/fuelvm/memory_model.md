# Memory Model

The EVM uses a linear memory (i.e. starting at zero and growing), without a defined limit. Allocating memory costs [a quadratic amount of gas](https://github.com/ethereum/yellowpaper/blob/8fea825c80e27fa9df5d89fb3365d1067788724e/Paper.tex#L2114), which severely limits the amount of memory usable in a single context (i.e. contract call).

In addition, the EVM has a separate memory space per context. Contexts may communicate with each other my copying data to call data and return data buffers.

The FuelVM uses a single share memory block _per transaction_. Memory is allocated statically with a known upper bound, allowing for straightforward implementation of heap types such as [vectors](https://github.com/FuelLabs/sway/blob/master/sway-lib-std/src/vec.sw). Memory in the FuelVM is globally readable across contexts, but locally writable. Each context may only write to portions of the stack and the heap which it has [ownership](https://github.com/FuelLabs/fuel-specs/blob/master/specs/vm/main.md#ownership) over.
