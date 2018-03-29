Memory management provides ways to dynamically allocate memory chunks for programs when they request it, and free them when they are no longer needed - so that they can be reused. The automatic memory management usually involves a garbage collector.

Managing your memory manually(like in C) can introduce several major bugs to your applications:
a. Memory leaks when the used memory space is never freed up.
b. Wild/dangling pointers appear when an object is deleted, but the pointer is reused. Serious security issues can be introduced when other data structures are overwritten or sensitive information is read.

The way how the Garbage Collector(GC) knows that objects are no longer in use is that no other object has references to them. But in order to decide what can be freed up, the GC consumes computing power.

New Space and Old Space
The heap has two main segments, the New Space and the Old Space. The New Space is where new allocations are happening; it is fast to collect garbage here and has a size of ~1-8MBs. Objects living in the New Space are called Young Generation.

The Old Space where the objects that survived the collector in the New Space are promoted into - they are called the Old Generation. Allocation in the Old Space is fast, however collection is expensive so it is infrequently performed .

Scavenge collection is fast and runs on the Young Generation, however the slower Mark-Sweep collection runs on the Old Generation.