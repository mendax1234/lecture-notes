# Thread API

In this section, we briefly introduce the POSIX thread API, focusing on how to use the `pthread` library.

{% hint style="warning" %}
`pthread` can be used to implement either **user** or **kernel** threads.
{% endhint %}

## Basics

In this section, we introduce the basics of the `pthread` library, including:

1. Thread Creation
2. Thread Termination
3. Thread Completion (Synchronization)

### Thread Creation

In POSIX, a thread can be created using `pthread_create()`:

{% code lineNumbers="true" expandable="true" %}
```c
#include <pthread.h>

int pthread_create(
    pthread_t *thread,
    const pthread_attr_t *attr,
    void *(*start_routine)(void *),
    void *arg
);
```
{% endcode %}

There are **four parameters**:

1. `thread`: A pointer to a `pthread_t` variable that stores the **ID of the newly created thread**. We can use it later to interact with the thread.
2. `attr`: Specifies the **attributes** of the thread, such as its stack size or scheduling priorities. Use `NULL` for the default attributes.
3. `start_routine`: Specifies the **function that the new thread will execute**.
4. `arg`: The **argument passed to** `start_routine` when the thread begins execution.

{% hint style="success" %}
The use of `void *` makes `start_routine` flexible: it can receive a pointer to **any type of data** as arguments and return a pointer to **any type of result**.
{% endhint %}

The `pthread_create` returns 0 to indicate success and `!0` to indicate errors.

### Thread Termination

In POSIX, a thread can be terminated using `pthread_exit()`:

```c
void pthread_exit(void *exitValue);
```

The parameter `exitValue` specifies the value returned to another thread that later **synchronizes with this thread** using `pthread_join()` (more on this later).

{% hint style="warning" %}
If `pthread_exit()` is not explicitly called, the thread automatically terminates when `start_routine` returns. In this case, there is no way to return an exit value.
{% endhint %}

<details>

<summary>A pthread creation &#x26; termination example</summary>

Suppose we have the following program,&#x20;

{% code lineNumbers="true" expandable="true" %}
```c
// header files not shown 

void* sayHello(void* arg) {
    printf("Just to say hello!\n");
    pthread_exit(NULL);
}

int main() {
    pthread_t tid;

    pthread_create(&tid, NULL, sayHello, NULL);
    printf("Thread created with tid %i\n", tid);

    return 0;
}
```
{% endcode %}

If we run this code, we may get the following output:

{% code lineNumbers="true" expandable="true" %}
```bash
danie:~$ ./thread
Thread created with tid 143296192
danie:~$ ./thread
Thread created with tid -268245312
Just to say hello!
```
{% endcode %}

Sometimes, we can see the output from a **child thread**, while other times we cannot. This is because when the **process terminates**, all threads belonging to that process are terminated as well, even if some have not finished executing.

{% hint style="success" %}
Threads created within a process are **part of that process**. Therefore, when the process terminates, **all of its threads terminate** as well.
{% endhint %}

If we want the **parent thread** to wait for its child threads to finish before terminating, we need to use **thread synchronization**, which we will introduce next!

</details>

### Thread Synchronization

In POSIX, thread synchronization is mainly done using `pthread_join()`, which allows one thread to **wait for another thread to finish**:

```c
int pthread_join(pthread_t thread, void **value_ptr);
```

It has two parameters:

1. `thread`: The ID of the target thread that the **calling thread** wants to wait for.
2. `value_ptr`: Stores the **exit value returned by the target thread**.

For example, in the following program, if we remove **lines 17–19**, the main thread may terminate before all five child threads finish, so we may not see the correctly updated value of `globalVar`.

{% code lineNumbers="true" expandable="true" %}
```c
// header files not shown 

int globalVar; 

void* doSum(void* arg) { 
    // same as before 
} 

int main() {
    pthread_t tid[5]; // 5 threads id 
    int i; 

    for (i = 0; i < 5; i++) 
        pthread_create(&tid[i], NULL, doSum, NULL); 

    // Wait for all threads to finish 
    for (i = 0; i < 5; i++) 
        pthread_join(tid[i], NULL); 

    printf("Global variable is %i\n", globalVar); 

    return 0; 
}
```
{% endcode %}

{% hint style="success" %}
Recap that threads within the same process **share the same address space**. Therefore, they can access and modify the same global variables.
{% endhint %}

### Locks

#### Mutex Locks

Beyond thread creation and `pthread_join()`, another important feature provided by the **POSIX Threads** library is **mutex locks**, which provide **mutual exclusion** for critical sections.

{% hint style="success" %}
For the purpose of NUS CG2271, we can treat a **mutex or lock as a binary semaphore**:
{% endhint %}

The basic usage of a mutex involves four steps:

1. **Declare** a mutex variable.
2. **Initialize** the mutex using `pthread_mutex_init()`.
3. **Lock and unlock** around the critical section using `pthread_mutex_lock()` and `pthread_mutex_unlock()`.
4. **Destroy** the mutex using `pthread_mutex_destroy()` when it is no longer needed.

{% hint style="success" %}
The `lock` and `unlock` method in step 3 are similar to the `wait()` and `signal()` method we've seen in [semaphores](../semaphores.md#semaphores).
{% endhint %}

For example:

{% code lineNumbers="true" expandable="true" %}
```c
#include <pthread.h>

pthread_mutex_t mutex;

int main()
{
    // Initialize mutex
    pthread_mutex_init(&mutex, NULL);

    // Enter critical section
    pthread_mutex_lock(&mutex);

    // Critical section:
    // access/modify shared resource

    // Exit critical section
    pthread_mutex_unlock(&mutex);

    // Destroy mutex when no longer needed
    pthread_mutex_destroy(&mutex);

    return 0;
}
```
{% endcode %}

If another thread calls `pthread_mutex_lock(&mutex)` while the mutex is already locked, it **waits until the current owner unlocks the mutex**, ensuring that only one thread executes the protected critical section at a time.

### Conditional Variables

So far, we have seen that **mutexes** can protect a critical section, but a mutex alone does not provide a convenient way for a thread to **wait until a particular condition becomes true**. A **conditional variable (condition variable)** provides a mechanism for a thread to **sleep until another thread signals that a condition may have changed**.

The key idea is:

* `mutex`: protects the shared data
* Conditional Variable: wait for a condition

A condition variable is therefore normally used **together with a mutex**.

#### **Basic Operations**

POSIX provides three main operations:

* `pthread_cond_wait()`: puts the calling thread to sleep until it is signalled. While waiting, it **temporarily releases the mutex** and automatically reacquires it before returning.
* `pthread_cond_signal()`: wakes **one** waiting thread.
* `pthread_cond_broadcast()`: wakes **all** waiting threads.

The typical pattern is:

{% code lineNumbers="true" expandable="true" %}
```c
pthread_mutex_lock(&mutex);

while (!condition) {
    pthread_cond_wait(&cond, &mutex);
}

// Condition is true, access shared data

pthread_mutex_unlock(&mutex);
```
{% endcode %}

Another thread can make the condition true and signal:

{% code lineNumbers="true" %}
```c
pthread_mutex_lock(&mutex);

// Modify shared data / make condition true

pthread_cond_signal(&cond);

pthread_mutex_unlock(&mutex);
```
{% endcode %}

The **condition** is usually represented by **shared state**, such as `count > 0`, which must be protected by a `mutex`. The **condition variable** provides a mechanism for a thread to sleep while the condition is false and be awakened when the shared state may have changed.

<details>

<summary>Solving the producer-consumer problems using conditonal variables</summary>

Let's see an example of using the conditional variables to wait for a buffer to become non-empty. Our consumer's code is like as follows:

{% code lineNumbers="true" expandable="true" %}
```c
pthread_mutex_lock(&mutex);

while (count == 0)
    pthread_cond_wait(&notEmpty, &mutex);

item = buffer[out];
out = (out + 1) % K;
count--;

pthread_mutex_unlock(&mutex);
```
{% endcode %}

The producer can signal the consumer after adding an item:

{% code lineNumbers="true" %}
```c
pthread_mutex_lock(&mutex);

buffer[in] = item;
in = (in + 1) % K;
count++;

pthread_cond_signal(&notEmpty);

pthread_mutex_unlock(&mutex);
```
{% endcode %}

Here, the consumer **does not busy-wait** on `count == 0`. Instead, it sleeps and gives up the CPU until the producer signals `notEmpty`.

{% hint style="success" %}
This example uses the `mutex` + conditonal variables method to solve the classic [producer-consumer problem](../semaphores.md#producer-consumer-problem).
{% endhint %}

</details>
