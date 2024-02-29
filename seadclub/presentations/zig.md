## Zig Programming Language
---
## Introduction
---
- General-purpose programming language and toolchain for maintaining robust, optimal, and reusable software, designed to be a system programming language.

- Attempts to be a modern replacement for C, focused on safety, performance, and compile-time execution. 

- Community-driven, it is developed in the open, and is a non-profit project, free to use under the MIT License. 
---
## History
---
- Was created by Andrew Kelley, and the first commit was made on April 1, 2016. 

- The first release was made on June 16, 2016. 

- Andrew Kelley is the main developer of Zig, and the project is maintained by a community of contributors.

- Before Zig was created, Andrew Kelley worked on the LDC project, a D compiler, and the Zig project was inspired by the LDC project. 
---
## Main features
---
- Safety

- Performance

- Debuggability

- Cross-compilation 

- No hidden control flow

- No hidden allocation 

- No macros 
---
- No header files 

- No dependencies 

- No runtime 

- No garbage collector

- No undefined behavior

- No global state 
---
## Code Examples
---
### Hello World
```zig
const std = @import("std");

pub fn main() void {
    const stdout = std.io.getStdOut().writer();
    try stdout.print("Hello, {}!\n", .{"World"});
}
```
---
### Fibonacci
```zig
const std = @import("std");

pub fn fib(n: u64) u64 {
    if (n <= 1) return n;
    return fib(n - 1) + fib(n - 2);
}

pub fn main() void {
    const stdout = std.io.getStdOut().writer();
    try stdout.print("fib(10) = {}\n", .{fib(10)});
}
```
---
### Factorial
```zig
const std = @import("std");

pub fn factorial(n: u64) u64 {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

pub fn main() void {
    const stdout = std.io.getStdOut().writer();
    try stdout.print("factorial(10) = {}\n", .{factorial(10)});
}
```
---
## Memory Management
---
- Zig has no garbage collector, and no hidden allocations.

- Memory management is done manually, and Zig has a built-in allocator.

- Zig has a built-in allocator, and it is possible to create custom allocators.
---
## Code Examples
---
### Allocating Memory
```zig
const std = @import("std");

pub fn main() void {
    const allocator = std.heap.page_allocator;
    const ptr = allocator.allocate(u8, 100);
    defer allocator.deallocate(ptr);
}
```
---
## Concurrency
---
## Zig has built-in support for
---
- Async/await:

```zig
const std = @import("std");

async fn sleep(ms: u64) void {
    const timer = std.time.Timer.now();
    while (timer.elapsed().milliseconds < ms) {
        await std.time.sleep(std.time.MilliSecond);
    }
}

async fn main() void { // async main function is allowed in Zig
    await sleep(1000);
    const stdout = std.io.getStdOut().writer();
    try stdout.print("Hello, World!\n");
}
```
---
- Coroutines:

```zig
const std = @import("std");

pub fn main() void {
    const allocator = std.heap.page_allocator;
    const coroutine = std.Coroutine.init(allocator);
    defer coroutine.destroy();
    coroutine.start(async {
        const stdout = std.io.getStdOut().writer();
        try stdout.print("Hello, World!\n");
    });
}
```
---
- Channels:

```zig
const std = @import("std");

pub fn main() void {
    const allocator = std.heap.page_allocator;
    const channel = std.Channel.init(u8, allocator);
    defer channel.destroy();
    const sender = channel.sender();
    const receiver = channel.receiver();
    sender.send(42);
    const value = receiver.receive();
}
```
---
- Mutexes:

```zig
const std = @import("std");

pub fn main() void {
    const mutex = std.Mutex.init;
    defer mutex.destroy();
    mutex.lock();
    defer mutex.unlock();
}
```
---
- Semaphores:

```zig
const std = @import("std");

pub fn main() void {
    const semaphore = std.Semaphore.init(1);
    defer semaphore.destroy();
    semaphore.acquire();
    defer semaphore.release();
}
```
---
- Condition variables:

```zig
const std = @import("std");

pub fn main() void {
    const mutex = std.Mutex.init;
    defer mutex.destroy();
    const cond = std.Cond.init;
    defer cond.destroy();
    mutex.lock();
    defer mutex.unlock();
    cond.wait(&mutex);
    cond.signal();
}
```
---
- Thread-local storage:

```zig
const std = @import("std");

pub fn main() void {
    const tls = std.ThreadLocalStorage.init(u8);
    defer tls.destroy();
    tls.set(42);
    const value = tls.get();
}
```
---
- Thread pools:

```zig
const std = @import("std");

pub fn main() void {
    const thread_pool = std.ThreadPool.init(4);
    defer thread_pool.destroy();
    thread_pool.spawn(async {
        const stdout = std.io.getStdOut().writer();
        try stdout.print("Hello, World!\n");
    });
}
```
---
- Atomic operations:

```zig
const std = @import("std");

pub fn main() void {
    const atomic = std.AtomicUsize.init(0);
    atomic.store(42);
    const value = atomic.load();
}
```
---
- Memory barriers:

```zig
const std = @import("std");

pub fn main() void {
    std.atomic.fence(std.AtomicOrder.SeqCst);
}
```
---
- Thread scheduling:

```zig
const std = @import("std");

pub fn main() void {
    const thread = std.Thread.init(async {
        const stdout = std.io.getStdOut().writer();
        try stdout.print("Hello, World!\n");
    });
    defer thread.join();
}
```
---
- Thread management:

```zig
const std = @import("std");

pub fn main() void {
    const thread = std.Thread.init(async {
        const stdout = std.io.getStdOut().writer();
        try stdout.print("Hello, World!\n");
    });
    defer thread.join();
}
```
---
- Thread safety:

```zig
const std = @import("std");

pub fn main() void {
    const atomic = std.AtomicUsize.init(0);
    atomic.store(42);
    const value = atomic.load();
}
```
