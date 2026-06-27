# EE4218 Embedded Hardware System Design

## Introduction

* **Full name**: [EE4218 Embedded Hardware System Design](https://nusmods.com/courses/EE4218/embedded-hardware-system-design)
* **Target audience**: NUS Year 4 EE/CEG Students
* **Purpose of the course**: To provide students with an amazing understanding of hardware/software co-design and the hands-on opportunity to design a modern AI accelerator.
* **Notes Content**: View the [EE4218 Notes](https://wenbo-notes.gitbook.io/ddca-notes/)

I took this course in AY25/26 Semester 2 mainly as a follow-up for the CG3207 that I took last sem.

## Course Content

### Overview of Topics Covered

1. **Introduction to Hardware and Embedded Systems:** This topic has quite a lot of overlap with Lecture 1 and Lecture 2 of CG3207, but it extends the discussion by introducing embedded systems and hardware/software co-design.
2. **Verilog for Synthesis:** This topic is almost the same as the second half of Lecture 2 in CG3207, covering the essential Verilog concepts needed for synthesizable hardware design.
3. **Sequential Logic Optimization:** This is a more advanced treatment of FSM design, covering topics such as state formulation, state minimization, retiming, and other optimization techniques.
4. **Abstract Models and Behavioural Optimisations:** This topic introduces hardware modelling techniques that abstract circuits into different types of graphs. I find this especially useful because such abstractions can serve as a bridge for communication between humans and machines.
5. **Microarchitecture Design:** As the name suggests, this topic teaches us how to approach microarchitecture design in a more systematic way. It is also interesting to connect it with the microarchitecture design I encountered in CG3207, where I saw how different submodules in the processor are organized and coordinated to execute instructions.
6. **High-Level Synthesis:** This topic introduces the techniques behind HLS, which many of us may have previously used in Vitis simply as one or several lines of code. It helps reveal what is actually happening behind those high-level commands.
7. **Timing:** This topic mainly covers setup time, hold time, timing constraints, and how to determine whether a circuit meets those constraints. It also explains how to estimate the best achievable performance of a design. In some sense, it feels like a more accessible version of part of the timing content covered in the first half of EE4415.
8. **Technology Mapping:** This is one of the most important steps happening behind the scenes when we click "Run Synthesis" in Vivado. It explains how a logical design is mapped onto the actual hardware resources available on the target device.
9. **Physical Synthesis:** This is one of the most important steps happening behind the scenes when we click "Run Implementation" in Vivado. It shows how the synthesized design is further transformed, placed, and optimized toward a physically realizable hardware implementation.

### Depth and Balance of Coverage

#### Theoretical Understanding

As Prof. Rajesh mentioned in the first lecture, this is a highly up-to-date course that teaches us how to use state-of-the-art tools such as Vivado and Vitis to design an AI accelerator from scratch. The course also provides us with a valuable overview of the ASIC design flow, including key steps such as logic synthesis and implementation, and explains how these processes work behind the scenes.

This course is also an excellent follow-up to CG3207. It allows students to further appreciate the beauty and elegance of RTL design through a more advanced and practical perspective.

#### Application and real-world examples

A key highlight of this course is its strong emphasis on the application and real-world examples. Building on the knowledge gained from the four labs, we will work on a hands-on AI accelerator design project, where we are given the freedom to choose the type of accelerator we want to implement.

{% hint style="success" %}
For our project, we designed a customized accelerator for CNN inference, called VNN, or Verilog Neural Network.&#x20;
{% endhint %}

In thsis project, we are required to compare the inference performance across three approaches:

* a pure CPU implementation,
* an HLS-based implementation, and
* our own HDL implementation.

Through this experience, we can gain a deeper understanding of AI accelerator design and better appreciate the importance of the usage of ASICs in modern AI applications.

#### Challenging or Unique Aspects

1. **Writing even better RTL Code**: Some people may think writing good RTL code is a challenge for this course. For those who have taken CG3207, I would say honestly that your RTL coding skill should be enough to deal with EE4218. For those who haven't taken CG3207, the RTL coding might be a bit difficult. But don't worry, Prof. Rajesh is always there for any help!
2. **Lecture Content**: Personally speaking, the lecture content sometimes is a little bit more difficult, especially when you are reviewing for the final exam. However, it is not impossible to conquer all the knowledge points (At least I believe reading my notes thoroughly and learn every knowledge point in it, you will be pretty much prepared for the finals unless Prof. Rajesh adds some new content).

## Teaching Style and Materials

### Teaching Style

#### Lecture

I don't have to say much here I guess, as Prof. Rajesh is the big boss of this course. He is the GOAT!

#### Lab

Compared to the four labs in CG3207, the four labs in this course are relatively manageable. Their main purpose is to help us build the necessary foundation and technical skills for the final project. This is especially important as the final project carries almost the same weightage as the four labs combined.

#### Project

As mentioned above, the project in EE4218 is definitely one of the highlights of this course! Very very up-to-date and very very interesting!

{% hint style="warning" %}
As far as I am concerned, the project for this course is likely to be changed in the future, as Prof. Rajesh also acknowledged to us that he is running out of new ideas on this project but will try to find more interesting stuff for future cohorts to do!
{% endhint %}

#### Assessments

1. **Quizzes**: For our cohort (AY25/26 Sem 2), the quizzes were all take-home and carried a relatively small weightage of 10% in total, with three quizzes altogether.
2. **Final**: The final assessment felt more like a way to evaluate our overall understanding of the course rather than simply a test of exam-taking skills. In my opinion, for those who have a solid grasp of the key concepts covered in my [notes](https://app.gitbook.com/o/MnEKr5A4lYXtOfhoXGj5/s/08HOWaEgI5q3ZZTecFRP/) will find the final both manageable and enjoyable.

### Course Book

Prof. Rajesh provides a bunch of textbooks recommended for this course during Lecture 1. Among all these textbooks, the one that relates most to this course and is the most worth reading is:

**Textbook**: _Synthesis and optimization of digital circuits_ by Giovanni De Micheli.

<figure><img src="../../.gitbook/assets/EE4218_Coursebook.png" alt=""><figcaption></figcaption></figure>

{% hint style="success" %}
The importance of this book to digital design is comparable to that of Hennessy and Patterson's book to computer architecture.
{% endhint %}

## Learning Experience

### Personal Insights

Overall, this was a very interesting and rewarding course for me. As a continuation of CG3207, it gave me the opportunity to apply the RTL design skills I had learned to something both meaningful and relevant to real-world applications.

The course also introduced me to the ASIC design flow, which I believe will be very useful for those who are interested in working in the chip design industry in the future!

### Skills Developed

This course deepened my understanding of embedded hardware system design and helped me develop several important skills:

1. Basic understanding of neural networks and convolutional neural networks,
2. Stronger RTL coding skills through a large-scale project,
3. Foundational knowledge of SoC design, such as the commonly used buses and interfaces like AXI.

## Workload and Time Management

* **Level of Difficulty**: 7/10 (for those who have taken CG3207) and 9/10 (for those who haven't taken CG3207)
* **Tips for future cohort**: I have open-sourced all my [lecture notes](https://app.gitbook.com/o/MnEKr5A4lYXtOfhoXGj5/s/08HOWaEgI5q3ZZTecFRP/), highlighting the most difficult concepts I encountered. In addition, I am currently working on a project which might get published on APCCAS 2026 (yes, it's the VNN project). Hope that the documentation that I have written for VNN will also help with learning EE4218!

## Conclusion

I would like to express my heartfelt thanks to Prof. Rajesh, my lab group mates, and everyone who discussed questions with me during the post-lecture Q\&A sessions. This course would not have been as enjoyable and meaningful without yall!

{% hint style="success" %}
Highly recommend to take this course together with EE4415!
{% endhint %}
