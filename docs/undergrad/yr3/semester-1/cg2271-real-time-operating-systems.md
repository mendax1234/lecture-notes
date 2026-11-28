# CG2271 Real-Time Operating Systems

## Introduction

* **Full name**: [CG2271 Real-Time Operating Systems](https://nusmods.com/courses/CG2271/real-time-operating-systems)
* **Target audience**: NUS Year 2 CEG Students
* **Purpose of the Course:** On one hand, to understand how operating systems actually work; on the other hand, to gain hands-on experience with ARM microcontroller programming (by which I mean learning how to control hardware peripherals, such as UART, by reading from and writing to their registers).
* **Notes Content**: View the [CG2271 Notes](https://app.gitbook.com/o/MnEKr5A4lYXtOfhoXGj5/s/U2H1IohCa1n4UnV3qHk8/)

I took this course in AY26/27 Semester 1 mainly to gain an understanding of the Operating Systems abstraction in the computer system! (Yo, the [8 abstractions from DDCA](https://app.gitbook.com/s/jTJFBPtKk6NwweAooH53/textbook/from-zero-to-one#abstraction) come here again!)

## Course Content

### Overview of Topics Covered

Following the structure of the famous textbook _Operating Systems: Three Easy Pieces (OSTEP)_, I think the topics covered in CG2271 can be divided into three main sections:

1. **Virtualization:**
   * **CPU Virtualization:** Processes, threads, context switching and scheduling.
   * **Memory Virtualization:** The OS memory system, virtual memory, multi-level page tables, and related concepts.
2. **Concurrency:** Synchronization issues that arise when multiple threads access shared data.
3. **Persistence:** File systems and how data is stored persistently.

### Depth and Balance of Coverage

#### Theoretical Understanding

Since AY25/26, CG2271 has been largely revamped by Prof. Colin, who is also the main professor behind CS2106. I would say CG2271 is now much better organized, and you actually get to learn how operating systems work! This is quite interesting and definitely good news for CEG undergraduates who want to build a solid understanding of OS concepts.

#### Application and real-world examples

That said, apart from the projects, which mainly require knowledge of synchronization (particularly semaphores), I feel that we have very few opportunities to actually apply the OS concepts learned in lectures during our hands-on lab sessions.

From what I heard from Prof. Colin, CS2106 students get to do actual POSIX programming in their labs, whereas CG2271 replaces this part with microcontroller programming on the MCXC444.

#### Challenging or Unique Aspects

To be honest, I would say OS is definitely not an easy topic (IYKYK if you've taken the AY26/27 Sem 1 CG2271 finals). In particular, I found the following topics quite challenging:

1. **Semaphores:** Designing semaphore-based solutions for real-world synchronization problems (beyond the standard examples like producer-consumer covered in lectures) and analyzing programs that use semaphores.
2. **Multi-level Page Tables:** This can be quite challenging at first, but it's still doable if you go through my notes carefully!

## Teaching Style and Materials

### Teaching Style

#### Lecture

Okay, Prof. Colin might be another GOATed professor in SoC! Even though only a handful of students attended lectures during my semester, I personally found his lectures very interesting, and you can really tell that he knows a lot about operating systems!

#### Tutorial

Most of the tutorial questions I guess are from CS2106 so the difficulty of the tutorial questions might be actually a bit high! But my advice will be to pay attention to the questions tha Prof. Colin discussed on his tutorial because they might appear in the midterms or finals at the end of the day!

#### Project

The project I worked on was an RTOS-based smart doorbell. Since the project is completely open-ended, coming up with a good idea is really important! That said, I'm really grateful to have met my CG2271 groupmates. Legit GOATed teammates!

{% hint style="success" %}
From what I've seen, most groups tend to get full marks, so there isn't much difference in project grades across groups.
{% endhint %}

#### Assessments

In AY26/27, our grading consisted of:

1. **Midterm:** From the results, most students did quite well (except me LOL).
2. **Project:** Most groups received nearly full marks.
3. **Final:** The hardest part, and where the real gap between students starts to show. Those who really understand OS concepts will ace it!

One tip from me is that Prof. Colin likes to include tricky tutorial questions in the midterms and finals. During my semester, both the scheduling and multi-level page table problems that once apperaed in the tutorials appeared in the exams!

{% hint style="warning" %}
However, I would suggest thinking twice before asking Prof. Colin for clarification during exams, as you might likely get a response of, "We've already covered this in tutorials. You should know this by now." So, my advice is to spend more time understanding the tricky tutorial questions beforehand rather than getting stressed out during the exams!
{% endhint %}

### Course Book

For OS, I actually relied quite a lot on the following textbook rather than the two recommended by Prof. Colin:

**Textbook:** _Operating Systems: Three Easy Pieces (OSTEP)_ by Remzi H. Arpaci-Dusseau and Andrea C. Arpaci-Dusseau.

<figure><img src="../../.gitbook/assets/CG2271_Coursebook" alt=""><figcaption></figcaption></figure>

{% hint style="warning" %}
Looking back, I do regret not spending more time studying the memory virtualization part, as I thought what I'd learned in CG3207 would be enough. But it turns out that it wasn't! Memory management in OS is way more complex than what I'd learned in CG3207!
{% endhint %}

## Learning Experience

### Personal Insights

My experience with this course has been really amazing, from the setback in the midterm to meeting my great groupmates for the project and finally doing reasonably okay in the finals. I really appreciate having had the chance to learn about OS from the perspectives of CPU virtualization, memory virtualization, concurrency, and file systems.

As OSTEP mentions in its [file system implementation section](https://app.gitbook.com/s/5cfVJRHhgDkjMTtQIPfW/file-system/file-system-implementation), building a mental model is essential to understanding a system. I do think these four areas together form a useful mental model for understanding how an OS works, which I find quite amazing!

### Skills Developed

As Prof. Colin mentioned in one of his lectures, after taking CG2271, we should understand at least two things:

1. **How an OS works:** An OS is essentially a program itself. On a single CPU core, when the OS is executing, user programs are not, and vice versa. The OS regains control through mechanisms such as interrupts, system calls, and exceptions.
2. **ARM Microcontroller Programming:** How to program peripheral devices, such as UART, by accessing their interface registers, including status, command/control, and data registers.

## Workload and Time Management

* **Level of Difficulty:** 7/10
* **Tips for future cohort**: I really really recommend to understand the OS in depth, either by reading textbooks or asking Prof. Colin, as OS is super important!

## Conclusion

Overall, I think the revamped CG2271 is quite well-organized, and I can really see that the CEG curriculum is getting better and better!
