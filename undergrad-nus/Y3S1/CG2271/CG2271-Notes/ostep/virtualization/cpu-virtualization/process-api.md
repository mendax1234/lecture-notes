---
description: This is just a FYI note to know how the actual UNIX shell works.
---

# Process API

In this section, we will discuss [process creation](the-process.md#process-creation-a-little-more-detail) in UNIX systems. UNIX presents one of the most intriguing ways to **create** a new process with a pair of [system calls](../../introduction-to-os.md#standard-library): `fork()` and `exec()`. A third routine, `wait()`, can be used by a process wishing to **wait** for a process it has created to complete. We now present these **interfaces** in more detail, with a few simple examples to motivate us.

{% hint style="warning" %}
The **interface** is nothing but another way of saying the **system call**.
{% endhint %}

<details>

<summary>Process API: A real world example</summary>

In this section, we will use **POSIX**, a standard interface that provides a set of system calls and APIs for interacting with the operating system. POSIX is commonly used on **UNIX and Linux** systems, while Windows provides its own equivalent called the **Windows API**.

{% hint style="success" %}
According to Prof. Colin, knowing POSIX programming is very very important!
{% endhint %}

</details>

## The `fork()` System Call

The `fork()` system call is used to create a new process. However, be forewarned:

> it is certainly the **strangest** routine we will ever call.

When a process calls `fork()`, the OS creates a new process by **copying the parent process's PCB**, including its **hardware, OS, and memory contexts**. The OS context is then updated with a **new** [**PID**](#user-content-fn-1)[^1], while the child continues executing the **same code from the point immediately after** `fork()`.

{% hint style="warning" %}
However, the child is **not an exact copy** of the parent. The return value of `fork()` differs between them:

* **Child process:** `fork()` returns `0`.
* **Parent process:** `fork()` returns the **PID of the newly created child**.

This allows the parent and child to determine which process they are executing in.
{% endhint %}

Let's examin the code snippet below.

{% code title="p1.c" lineNumbers="true" expandable="true" %}
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[])
{
    printf("hello world (pid:%d)\n", (int)getpid());
    int rc = fork();
    if (rc < 0)
    {
        // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    }
    else if (rc == 0)
    {
        // 0: child process (new process)
        printf("hello, I am child (pid:%d)\n", (int)getpid());
    }
    else
    {
        // non-zero: parent process
        // parent goes down this path (original process)
        printf("hello, I am parent of %d (pid:%d)\n",
               rc, (int)getpid());
    }
    return 0;
}
```
{% endcode %}

When we run this program (`p1.c`), we'll see the following:

{% code lineNumbers="true" expandable="true" %}
```bash
danie@DESKTOP-IAI9BIM:~$ ./p1
hello world (pid:483)
hello, I am parent of 484 (pid:483)
hello, I am child (pid:484)
```
{% endcode %}

Thus, the exact flow of this program is that:

1. **Parent prints the first message:** The original process starts at `main()`, prints `"hello world (pid:...)"`, and moves to the next line.
2. **Parent enters** `fork()`**:** The parent executes `int rc = fork();`. The OS pauses the parent and creates an exact clone (the child).
3. **The split happens:** The OS copies the memory and the Program Counter. At this exact millisecond, there are two identical processes sitting inside the kernel, waiting to finish the `fork()` function.
4. **Both processes return:** The OS resumes the processes. They **both** "return from the `fork()` system call" and assign a value to `rc`. This is where the magic happens: the OS feeds a different return value to each process so they know who they are. The parent's `fork()` returns the child's PID. The child's `fork()` returns `0`.
5. **Execution diverges:** Both processes move down to the `if (rc < 0)` statement. Because their `rc` values are different, the child falls into the `else if (rc == 0)` block, and the parent falls into the `else` block (We deliberately write so because we don't want the parent and child process to execute the same thing!)

<details>

<summary>Non-Determinism</summary>

The output of `p1.c` is **not deterministic**. After `fork()`, both the **parent and child are ready to run**. On a single-CPU system, either process may run first, so the order of their output can vary between executions. The following output trace is also possible for `p1.c`!

{% code lineNumbers="true" expandable="true" %}
```c
danie@DESKTOP-IAI9BIM:~$ ./p1
hello world (pid:483)
hello, I am child (pid:484)
hello, I am parent of 484 (pid:483)
```
{% endcode %}

The **CPU scheduler** decides which process runs at each moment. Since we generally cannot predict its decisions, we cannot assume which process will execute first.

{% hint style="success" %}
Scheduling is **not deterministic**! This **non-determinism** becomes especially important in **multithreaded programs**, where multiple threads may execute in different orders and interact in unexpected ways. We will explore these issues further when we study **concurrency**.
{% endhint %}

</details>

## The `wait()` System Call

So far, we haven't done much: just created a child that prints out a message and exits. Sometimes, as it turns out, it is quite useful for a parent to wait for a child process to finish what it has been doing. This task is accomplished with the `wait()` system call (or its more complete sibling `waitpid()`); see the following code snippet for details.

{% code title="p2.c" lineNumbers="true" expandable="true" %}
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(int argc, char *argv[])
{
    printf("hello world (pid:%d)\n", (int)getpid());
    int rc = fork();
    if (rc < 0)
    {
        // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    }
    else if (rc == 0)
    {
        // child (new process)
        printf("hello, I am child (pid:%d)\n", (int)getpid());
        sleep(1);
    }
    else
    {
        // parent goes down this path (original process)
        int wc = wait(NULL);
        printf("hello, I am parent of %d (wc:%d) (pid:%d)\n",
               rc, wc, (int)getpid());
    }
    return 0;
}
```
{% endcode %}

In this example (`p2.c`), the parent process calls `wait()` to delay its execution until the child finishes executing. When the child is done, `wait()` returns to the parent.

{% hint style="success" %}
The return value of `wait()` is the **PID of the child process that was reaped**. If a parent has multiple child processes, `wait()` may return the PID of **any one of the children that has terminated**. For more practical usage of this hint, please go through [this question](https://app.gitbook.com/s/U2H1IohCa1n4UnV3qHk8/tut/tut-02-the-process-api#id-3.-parallel-computation) covered in NUS CG2271 Tutorial 2!
{% endhint %}

Adding a `wait()` call to the code above makes the output **deterministic.** Here is the output:

{% code lineNumbers="true" expandable="true" %}
```bash
danie@DESKTOP-IAI9BIM:~$ ./p2
hello world (pid:752)
hello, I am child (pid:753)
hello, I am parent of 753 (wc:753) (pid:752)
```
{% endcode %}

With this code, we now know that the child will always print first. Why do we know that? Well, it might simply run first, as before, and thus print before the parent. However, if the parent does happen to run first, it will immediately call `wait()`; this system call won't return until the child has run and exited[^2]. Thus, even when the parent runs first, it politely waits for the child to finish running, then `wait()` returns, and then the parent prints its message.

<details>

<summary>Zombine Process</summary>

After a child process finishes execution, its [**exit status**](#user-content-fn-3)[^3] is stored by the OS in the child process's PCB so that the parent can retrieve it using `wait()` (or `waitpid()`). Until then, the terminated child remains in the process table as a **zombie process**.

Therefore, the parent should always **reap its terminated children** using `wait()` or `waitpid()` to prevent zombie processes from accumulating.

</details>

## The `exec()` System Call

A final and important piece of the process creation API is the `exec()` system call. This system call is useful when we want to run a program that is **different** from the calling program. For example, calling `fork()` in `p2.c` is only useful if we want to keep running copies of the **same** program. However, often we want to run a different program; `exec()` does just that (See the code snippet below).

{% hint style="warning" %}
Actually, there are six variants of `exec()`: `execl()`, `execle()`, `execlp()`, `execv()`, and `execvp()`. Read the man pages to learn more.
{% endhint %}

{% code title="p3.c" lineNumbers="true" expandable="true" %}
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main(int argc, char *argv[])
{
    printf("hello world (pid:%d)\n", (int)getpid());
    int rc = fork();
    if (rc < 0)
    {
        // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    }
    else if (rc == 0)
    {
        // child (new process)
        printf("hello, I am child (pid:%d)\n", (int)getpid());
        char *myargs[3];
        myargs[0] = strdup("wc");   // program: "wc" (word count)
        myargs[1] = strdup("p3.c"); // argument: file to count
        myargs[2] = NULL;           // marks end of array
        execvp(myargs[0], myargs);  // runs word count
        printf("this shouldn't print out");
    }
    else
    {
        // parent goes down this path (original process)
        int wc = wait(NULL);
        printf("hello, I am parent of %d (wc:%d) (pid:%d)\n",
               rc, wc, (int)getpid());
    }
    return 0;
}
```
{% endcode %}

In this example, the child process calls `execvp()` in order to run the program `wc`, which is the word counting program. In fact, it runs `wc` on the source file `p3.c`, thus telling us how many lines, words, and bytes are found in the file:

{% code lineNumbers="true" expandable="true" %}
```bash
danie@DESKTOP-IAI9BIM:~$ ./p3
hello world (pid:815)
hello, I am child (pid:816)
  36  120 1026 p3.c
hello, I am parent of 816 (wc:816) (pid:815)
```
{% endcode %}

The `fork()` system call is strange; its partner in crime, `exec()`, is not so normal either. What it does:

1. given the name of an executable (e.g., `wc`), and some arguments (e.g., `p3.c`), it loads code (and static data) from that executable and
2. **overwrites** its current code segment (and current static data) with it; the heap and stack and other parts of the memory space of the program are re-initialized.
3. Then the OS simply runs that program, passing in any arguments as the `argv` of that process.

Thus, it does not create a new process; rather, it **transforms** the currently running program (formerly p3) into a different running program (`wc`). After the `exec()` in the child, it is almost as if `p3.c` never ran.

> &#x20;a successful call to `exec()` **never returns**

The line `printf("this shouldn't print out");` has been physically overwritten by the code for the word count program. The OS simply points the CPU's Program Counter to the entry point of `wc` and lets it run. When `wc` is finished counting words, it calls `exit()`, and the child process terminates. It never goes back to the `p3` code because there is no `p3` code left.

{% hint style="danger" %}
Because `exec()` destroys **memory**, but it does not destroy **OS-level resources** like open files. This is the whole reason UNIX separates `fork()` and `exec()`. In the [gap](process-api.md#whats-exactly-the-gap) between the two, the child can open files, close files, or reroute its standard input/output. When `exec()` wipes the memory and loads the new program, the new program inherits those modified file connections without even realizing it.
{% endhint %}

## Why? Motivating the API

Of course, one big question you might have: why would we build such an odd interface to what should be the simple act of creating a new process? We will use two examples to demonstrate the usefulness of the structure used in our previous code snippets (`fork()` + `exec()`).

1. UNIX Shell
2. UNIX Pipe

### UNIX Shell

Well, as it turns out, the separation of `fork()` and `exec()` is essential in building a **UNIX shell**, because it lets the shell run code _after_ the call to `fork()` but _before_ the call to `exec()` (the **gap**); this code can alter the environment of the about-to-be-run program, and thus enables a variety of interesting features to be readily built.

<details>

<summary>What's exactly the gap?</summary>

The gap is a brief window of time where the child process exists, but it hasn't yet pressed the `exec()` self-destruct button to transform into the new program. More specifically, it refers to the space inside the child's `if` block:

{% code lineNumbers="true" expandable="true" %}
```c
else if (rc == 0) 
    {
        // 1. WE ARE AFTER FORK()
        // The child process has just been born. 
        
        // ---> THIS IS THE GAP <---
        // Any code we write here runs in the child process, 
        // but the child is still running the original shell code.

        // 2. WE ARE BEFORE EXEC()
        execvp(myargs[0], myargs); 
        
        // (Memory is wiped; the new program takes over)
    }
```
{% endcode %}

If `fork()` and `exec()` were combined into a single step, the shell wouldn't have a place to insert this setup code. The OS designers would have had to build a massive, complex `create_process_and_redirect_files_and_change_users()` function to handle every possible configuration a shell might want to do. By splitting them up, the OS stays simple, and the shell gets the power to build complex features using basic C code.

</details>

The shell is just a user program. It shows us a prompt and then waits for us to type something into it. We then type a command (i.e., the name of an executable program, plus any arguments) into it; in most cases, the shell then figures out where in the file system the executable resides, calls `fork()` to create a new child process to run the command, calls some variant of `exec()` to run the command, and then waits for the command to complete by calling `wait()`. When the child completes, the shell returns from `wait()` and prints out a prompt again, ready for our next command.

The separation of `fork()` and `exec()` allows the shell to do a whole bunch of useful things rather easily. For example:

{% code lineNumbers="true" expandable="true" %}
```bash
danie@DESKTOP-IAI9BIM:~$ wc p3.c > newfile.txt
```
{% endcode %}

In the example above, the output of the program `wc` is **redirected** into the output file `newfile.txt` (the greater-than sign is how said redirection is indicated). The way the shell accomplishes this task is quite simple: when the child is created, before calling `exec()`, the shell closes standard output and opens the file `newfile.txt`. All of these are done in the [**gap**](process-api.md#whats-exactly-the-gap) that we mentioned above and this demonstrates the **powerfulness** of this `fork()` + `exec()` design! By doing so, any output from the soon-to-be-running program `wc` is sent to the file instead of the screen. The following code snippet shows a program that does exactly this.

{% code title="p4.c" lineNumbers="true" expandable="true" %}
```c
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <fcntl.h>
#include <assert.h>
#include <sys/wait.h>

int main(int argc, char *argv[])
{
    int rc = fork();
    if (rc < 0)
    {
        // fork failed; exit
        fprintf(stderr, "fork failed\n");
        exit(1);
    }
    else if (rc == 0)
    {
        // child: redirect standard output to a file
        close(STDOUT_FILENO);
        open("./p4.output", O_CREAT | O_WRONLY | O_TRUNC, S_IRWXU);

        // now exec "wc"...
        char *myargs[3];
        myargs[0] = strdup("wc");   // program: "wc" (word count)
        myargs[1] = strdup("p4.c"); // argument: file to count
        myargs[2] = NULL;           // marks end of array
        execvp(myargs[0], myargs);  // runs word count
    }
    else
    {
        // parent goes down this path (original process)
        int wc = wait(NULL);
        assert(wc >= 0);
    }
    return 0;
}
```
{% endcode %}

The reason this redirection works is due to an assumption about how the operating system manages file descriptors. Specifically, UNIX systems start looking for free file descriptors at zero. In this case, `STDOUT_FILENO` will be the first available one and thus get assigned when `open()` is called. Subsequent writes by the child process to the standard output file descriptor, for example by routines such as `printf()`, will then be routed transparently to the newly-opened file instead of the screen.

Here's the output of running the `p4.c` program:

{% code lineNumbers="true" expandable="true" %}
```bash
danie@DESKTOP-IAI9BIM:~i$ ./p4
danie@DESKTOP-IAI9BIM:~$ cat p4.output 
 38 118 980 p4.c
```
{% endcode %}

We'll notice (at least) two interesting tidbits about this output.

1. First, when `p4` is run, it looks as if nothing has happened; the shell just prints the command prompt and is immediately ready for your next command. However, that is not the case; the program p4 did indeed call `fork()` to create a new child, and then run the `wc` program via a call to `execvp()`. We don't see any output printed to the screen because it has been redirected to the file `p4.output`.
2. Second, we can see that when we cat the output file, all the expected output from running `wc` is found. Cool, right?

{% hint style="success" %}
The `p4.c` program gives us a glimpse into how a **UNIX shell works internally**, demonstrating the power of combining **`fork()` and `exec()`**.
{% endhint %}

### UNIX Pipe

> We first encountered UNIX pipes in [NUS CS1010](https://app.gitbook.com/s/KipySCGxC8NC1UpA24DS/lec-tut-lab-exes/lecture/lec-03-basic-c-programming#standard-input-and-standard-output); now we will explore how their implementation relates to the `fork()` + `exec()` pattern introduced earlier.

UNIX pipes are implemented in a similar way, but with the `pipe()` system call. In this case, the output of one process is connected to an in-kernel **pipe** (i.e., queue), and the input of another process is connected to that same pipe; thus, the output of one process seamlessly is used as input to the next, and long and useful chains of commands can be strung together. As a simple example, consider looking for a word in a file, and then counting how many times said word occurs; with pipes and the utilities `grep` and `wc`, it is easy — just type `grep -o foo file | wc -l` into the command prompt and marvel at the result.

{% hint style="success" %}
Finally, while we just have sketched out the process API at a high level, there is a lot more detail about these calls out there to be learned and digested; we'll learn more, for example, about file descriptors when we talk about file systems in the third part of the book. For now, suffice it to say that the `fork()`/`exec()` combination is a **powerful** way to create and manipulate processes.
{% endhint %}

## Other Parts Of The API

Beyond `fork()`, `exec()`, and `wait()`, there are a lot of other interfaces for interacting with processes in UNIX systems. For example, the `kill()` system call is used to send signals to a process, including directives to go to sleep, die, and other useful imperatives. In fact, the entire signals subsystem provides a rich infrastructure to deliver external events to processes, including ways to receive and process those signals.

There are many command-line tools that are useful as well. For example, using the `ps` command allows us  to see which processes are running; read the man pages for some useful flags to pass to `ps`. The tool `top` is also quite helpful, as it displays the processes of the system and how much CPU and other resources they are eating up. Humorously, many times when we run it, `top` claims it is the top resource hog; perhaps it is a bit of an egomaniac. Finally, there are many different kinds of CPU meters you can use to get a quick glance understanding of the load on your system.

<details>

<summary>RTFM — Read The Man Pages</summary>

Many times in this book, when referring to a particular system call or library call, we'll tell you to read the **manual pages**, or **man pages** for short. Man pages are the original form of documentation that exist on UNIX systems; realize that they were created before the thing called **the web** existed.

Spending some time reading man pages is a key step in the growth of a systems programmer; there are tons of useful tidbits hidden in those pages. Some particularly useful pages to read are the man pages for whichever shell you are using (e.g., `tcsh`, or `bash`), and certainly for any system calls your program makes (in order to see what return values and error conditions exist).

Finally, reading the man pages can save you some embarrassment. When you ask colleagues about some intricacy of `fork()`, they may simply reply: "RTFM." This is your colleagues' way of gently urging you to Read The Man pages. The F in RTFM just adds a little color to the phrase...

</details>

## Summary

We have introduced some of the APIs dealing with UNiX process creation: `fork()`, `exec()` and `wait()`. However, we have just skimmed the surface. For more detail, please wait until we cover more chapters later! There is much to extract from the wisdom therein.

[^1]: This is known as the **process identifier** of a process.

[^2]: There are a few cases where `wait()` returns before the child exits; read the man page for more details, as always.

[^3]: The **exit status** is an integer returned when a process terminates. A value of **0** usually indicates successful execution, while a **non-zero value** is typically used to indicate an error or other termination status. For example, `return 0;` in `main()` results in an exit status of 0, while `return n;` can return a different integer value.
