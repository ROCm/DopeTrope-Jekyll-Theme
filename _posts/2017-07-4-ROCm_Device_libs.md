---
title: ROCm Compiler Device Libraries
featured: /assets/images/pic02.jpg
layout: post
---

### ROCm Device Libraries Now Available!
We are pleased to announce fully open-source bitcode device libraries for the ROCm platform.

The following libraries are available:

* Open Compute Math Library (OCML)—contains 250+ optimized math functions for single, double and half precision. See the OCML documentation for a list of currently supported functions.
* Open Compute Kernel Library (OCKL)—contains many useful functions, including work-item/workgroup functions, integer optimization operations and wavefront reduce algorithms. It also serves as a foundation for the built-in library of the OpenCL-enabled compiler.
* Compiler built-in library to support OpenCL.
* Compiler built-in library for the HC mode of the Heterogeneous Compute Compiler (HCC).
* OCLC libraries—enable fine-grain control over library modes (such as relaxed math).
* IRIF—interface library to AMD GPU back end.

The libraries are intended for language and run-time implementors. They work with the open-source AMD GPU LLVM back end on GCN architectures and link to user bitcode through the LLVM linker before code generation.

ROCm-Device-Libs is currently under development, so we expect many improvements. As always, contributions and other feedback are welcome.
