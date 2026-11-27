# Memory Virtualization

{% hint style="info" %}
This part is essentially [Lec 7](https://app.gitbook.com/s/U2H1IohCa1n4UnV3qHk8/lec/lec-07-memory-abstraction) to [Lec 9](https://app.gitbook.com/s/U2H1IohCa1n4UnV3qHk8/lec/lec-09-virtual-memory-management) from NUS CG2271! But it will be a bonus if can add notes from OSTEP!
{% endhint %}

## Big Picture

From our previous learning, we know that a process's **memory context** consists of its **code, data, heap, and stack**. When a program starts, the OS loads the necessary parts of its memory context from disk into physical memory so that the process can execute. In this section, we will essentially learn **how a process's virtual memory is mapped to physical memory** and what happens behind the scenes when the process executes.

We will first study **contiguous memory allocation**, where each process occupies a single continuous region of physical memory. Under this approach, we will explore three memory allocation schemes:

1. **Fixed-Size Partitioning:** Physical memory is divided into fixed-size partitions, and each process can occupy at most one partition.
2. **Variable-Size Partitioning:** Physical memory is allocated dynamically according to the memory required by each process.
3. **Buddy System:** Physical memory is recursively divided into blocks whose sizes are powers of two, allowing memory to be allocated and merged efficiently.

However, these schemes can suffer from **internal and/or external fragmentation**.

> In particular, external fragmentation arises because a process requires a contiguous region of physical memory, even when sufficient free memory exists across multiple non-contiguous regions.

To address this problem, we introduce **paging**, a form of **non-contiguous/disjoint memory allocation**. Paging divides a process's virtual address space into fixed-size **pages** and physical memory into equally sized **frames**. The OS maintains a **page table** in the main memory for each process to map its virtual pages to physical frames, allowing a process's pages to be stored anywhere in physical memory.

* Under non-contiguous/disjoint memory allocation, we will also explore **segmentation with paging**. Instead of treating a process's virtual address space as one continuous region, we divide it into logical segments, such as code, data, heap, and stack. Each segment has its own page table, and a **segment table** maintains information about these segments and their corresponding page tables.

However, paging introduces another problem:

> Recall that under **memory virtualization**, each process appears to have its own large virtual address space. With a conventional single-level page table, we need an entry for every virtual page, even if much of the virtual address space is unused. This can make the page table extremely large! Moreover, a conventional single-level page table typically requires a contiguous region of physical memory.

To address both problems, we introduce **multi-level paging**. The basic idea is simple: instead of storing one huge page table, we divide it into smaller page tables that can be allocated separately in physical memory.

> But how small should these page tables be?

Typically, we design each page table to occupy **exactly one physical frame**. After dividing the original page table into smaller tables, we need another table to keep track of them. This higher-level page table contains entries that point to the page tables at the next level.

If this higher-level table is still too large to fit into one frame, we introduce another level. We repeat this process until we have a single **root page table**.

Thus, a multi-level page table has a **tree-like structure**, where each non-leaf entry points to a page table at the next level, and the leaf entries map virtual pages to physical frames!

> But how does this solve the problem of excessive page table memory usage?

The key observation is that **we only need to allocate the lower-level page tables that are actually required**, rather than allocating entries for the entire virtual address space.

> Why?

Recall that page table entries contain a **valid bit**. Similarly, entries in higher-level page tables indicate whether the corresponding lower-level page table exists. If an entry is invalid, the entire subtree beneath it can be omitted, meaning that we do not need to allocate any physical memory for those lower-level page tables.

Therefore, multi-level paging reduces memory usage by **avoiding the allocation of page tables for unused regions of the virtual address space**. At the same time, since each page table occupies only one frame, the page tables no longer need to be stored contiguously in physical memory.

In essence, **multi-level paging applies the idea of paging to the page table itself**, solving both the excessive memory usage and contiguous allocation problems associated with conventional single-level page tables.
