# Tutorial for TinyChat: Optimizing LLM on Edge Devices

This is a lab for [efficientml.ai course](https://efficientml.ai/).

Running large language models (LLMs) on the edge is of great importance. By embedding LLMs directly into real-world systems such as in-car entertainment systems or spaceship control interfaces, users can access instant responses and services without relying on a stable internet connection. Moreover, this approach alleviates the inconvenience of queuing delays often associated with cloud services. As such, running LLMs on the edge not only enhances user experience but also addresses privacy concerns, as sensitive data remains localized and reduces the risk of potential breaches.

However, despite their impressive capabilities, LLMs have traditionally been quite resource-intensive. They require considerable computational power and memory resources, which makes it challenging to run these models on edge devices with limited capabilities.

In this lab, you will learn the following:
* How to deploy an LLaMA2-7B-chat with TinyChatEngine on your computer.
* Implement different optimization techniques (loop unrolling, multithreading, and SIMD programming) for the linear kernel.
* Observe the end-to-end latency improvement achieved by each technique.

## Implement (x86 arch)
- [x] loop_unrolling
- [x] multithreading
- [x] simd_programming
- [x] multithreading_loop_unrolling
- [x] all_techniques

## Benchmark

The naive implemenation can be extremely slow. You may have to wait more than 10 minutes to get llama2's answer.

I provide my benchmarks of each accerleration implementation on two devices.

**MacBook Pro (15-inch, 2018)**

- Arch x86
- CPU: 6 Intel i7 Cores (2.2 GHz)
- Mem: 16 GB 2400 MHz DDR4

```
➭ ./evaluate.sh reference
-------- Sanity check of reference implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
reference, 3377.129150, 337.712006, 10, 0.776233

➭ ./evaluate.sh loop_unrolling
-------- Sanity check of loop_unrolling implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
loop_unrolling, 3335.317139, 333.531006, 10, 0.785964


➭ ./evaluate.sh multithreading
-------- Sanity check of multithreading implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
multithreading, 887.180054, 88.718002, 10, 2.954801


➭ ./evaluate.sh simd_programming
-------- Sanity check of simd_programming implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
simd_programming, 2073.361084, 207.336014, 10, 1.264343


➭ ./evaluate.sh multithreading_loop_unrolling
-------- Sanity check of multithreading_loop_unrolling implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
multithreading_loop_unrolling, 838.462036, 83.846001, 10, 3.126486


➭ ./evaluate.sh all_techniques               
-------- Sanity check of all_techniques implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
all_techniques, 227.922012, 22.792002, 10, 11.501479
```

**Linux Desktop Computer**

- Arch: x86
- CPU: AMD Ryzen 9 5900X 12-Core (3.7GHz)
- Mem: 64 GB 2400 MHz DDR4


```
$ ./evaluate.sh reference
-------- Sanity check of reference implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
reference, 2726.144043, 272.614014, 10, 0.961593


$ ./evaluate.sh loop_unrolling
-------- Sanity check of loop_unrolling implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
loop_unrolling, 2177.221924, 217.722000, 10, 1.204030


$ ./evaluate.sh multithreading
-------- Sanity check of multithreading implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
multithreading, 720.249023, 72.024002, 10, 3.639630


$ ./evaluate.sh simd_programming
-------- Sanity check of simd_programming implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
simd_programming, 1852.494995, 185.248993, 10, 1.415086


$ ./evaluate.sh multithreading_loop_unrolling
-------- Sanity check of multithreading_loop_unrolling implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
multithreading_loop_unrolling, 570.304993, 57.029999, 10, 4.596558


$ ./evaluate.sh all_techniques
-------- Sanity check of all_techniques implementation: Passed! -------- 
Section, Total time(ms), Average time(ms), Count, GOPs
all_techniques, 177.613007, 17.761000, 10, 14.759280

```


## TinyChatEngine

This tutorial is based on [TinyChatEngine](https://github.com/mit-han-lab/TinyChatEngine), a powerful neural network library specifically designed for the efficient deployment of quantized large language models (LLMs) on edge devices. 

![demo](assets/figures/chat.gif)

## Tutorial document

Please check this document and follow the instructions which will walk you through the tutorial: https://docs.google.com/document/d/13IaTfPKjp0KiSBEhPdX9IxgXMIAZfiFjor37OWQJhMM/edit?usp=sharing

## Submission

* Report: Please write a report ([form](https://docs.google.com/document/d/17Z_ab8EhDvjcigLXdDqMqd2LTVsZ4CnpOYNkRTrnTmU/edit?usp=sharing)) that includes your code and the performance improvement for each starter code. 
* Code: Use `git diff` to generate a patch for your implementation. We will use this patch to test the correctness of your code. Please name your patch as `{studentID}-{ISA}.patch` where {ISA} should be one of x86 and ARM, depending on your computer.

## Related Projects

[TinyChatEngine](https://github.com/mit-han-lab/TinyChatEngine).

[TinyEngine](https://github.com/mit-han-lab/tinyengine).

[Smoothquant](https://github.com/mit-han-lab/smoothquant).

[AWQ: Activation-aware Weight Quantization for LLM Compression and Acceleration](https://github.com/mit-han-lab/llm-awq)

## Acknowledgement

[llama.cpp](https://github.com/ggerganov/llama.cpp)

[transformers](https://github.com/huggingface/transformers)
