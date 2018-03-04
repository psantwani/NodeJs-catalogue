# Principles
It's the OS that does the hard work of scheduling our applications and interleaving it with I/O, so it is essential that we understand the principles. Concurrent code has a bad reputation of being notoriously easy to screw up. Data races are not the only problem, though: inefficient locking, starvation, etc also arise.

# Difference
Concurrency is much broader, general problem than parallelism. If you have tasks having inputs and outputs, and you want to schedule them so that they produce correct results, you are solving a concurrency problem.

Lets say task1 takes the i/p, task 2,3,4 process them and task5 outputs them. We can run 2,3,4 concurrently after t1. Since there is no specific order between them, we can run them in jumbled order sequentially => 2,4,3; 3,2,4; etc. We can also run them in paraller if we have multiple processors(cores). So if we have only one processor, why even write concurrent apps? The processing time is not getting short, also we add the overhead of scheduling time.

Reasons -
We like to multi-task with computers - music, typing, getting notifications, etc. Not all operations are carried out on computer's CPU. Eg. When writing to disk, most time is spent in seeking position and writing to sectors, etc and intermittent time can be spent in doing something else. These requires the OS kernel to run tasks in an interleved manner, called time sharing.

# Process and threads
A process is a running instance of a computer program. It is what you see in the task manager of your operating system or top. A process consists of allocated memory which holds the program code, its data, a heap for dynamic memory allocations, and a lot more. However ,it is not the unit for multi-tasking in desktop operating systems.

Thread is the default unit - the task - of CPU usage. Code executed in a single thread is what we usually refer to as sequential or synchronous execution. They have their own call stacks, virtual CPU and (often) local storage but share the application's heap, data, codebase and resources (such as file handles) with the other threads in the same process. They also serve as the unit of scheduling in the kernel. For this reason, we call them kernel threads, clarifying that they are native to the operating system and scheduled by the kernel, which distinguishes them from user-space threads, also called green threads, which are scheduled by some user space scheduler such as a library or VM.

```
“preemption is the act of temporarily interrupting a task being carried out by a computer system, without requiring its cooperation, and with the intention of resuming the task at a later time” -
```

Context switching (switching between threads) is done at frequent intervals by the kernel, creating the illusion that our programs are running in parallel, whereas in reality, they are running concurrently but sequentially in short slices. 

# CPU vs I/O
Programs usually don't only consist of numeric, arithmetic and logic computations, in fact, a lot of times they merely write something to the file system, do network requests or access peripheries such as the console or an external device.
While the first kind of workload is CPU intensive, the latter requires performing I/O in the majority of the time.

When an I/O operation is requested with a blocking system call, we are talking about blocking I/O. This means that all threads in a process share a common kernel thread, which implies that every thread is blocked when one does blocking I/O. No wonder that modern OSes don't do this. Instead, they use one-to-one mapping, i.e. map a kernel thread to each user-space thread, allowing another thread to run when one makes a blocking system call.

# I/O flavors: Blocking vs. non-blocking, sync vs. async
Doing I/O usually consists of two distinct steps:

checking the device:
blocking: waiting for the device to be ready, or
non-blocking: e.g. polling periodically until ready, then

transmitting:
synchronous: executing the operation (e.g. read or write) initiated by the program, or
asynchronous: executing the operation as response to an event from the kernel (asynchronous / event driven)

You can mix the two steps in every fashion. Synchronous, blocking I/O has wide support with long established POSIX interfaces and is the most widely understood and easy to use. Its drawback is that you have to rely on thread-based concurrency, which is sometimes undesirable:
a. every thread allocated uses up resources
b. more and more context switching will happen between them
c. the OS has a maximum number of threads.

That's why modern web servers shifted to the async non-blocking model, and advocate using a single-threaded event loop for the network interface to maximize the throughput.
