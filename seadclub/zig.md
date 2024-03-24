- [Ziglang](#ziglang)
- [Introduction](#introduction)
- [History](#history)
- [Features](#features)
- [Code examples](#code-examples)
- [Memory management](#memory-management)
- [Code examples](#code-examples)
- [Parallelism](#parallelism)
- [Zig has built-in support for:](#zig-has-built-in-support-for)
---
## Ziglang
---
## Introduction
---
- Загальнопризначена мова програмування та інструментарій для створення міцного, оптимального та повторно використовуваного програмного забезпечення, призначена бути мовою системного програмування.
---
- Спрямована на те, щоб бути сучасною альтернативою мові C, з фокусом на безпеці, продуктивності та виконанні на етапі компіляції.
---
- Розвивається спільнотою, відбувається відкритий розробка і є некомерційним проектом, доступним для використання за ліцензією MIT.
--
## History
---
- Створено Ендрю Келлі, перший коміт був здійснений 1 квітня 2016 року.

- Перший реліз відбувся 16 червня 2016 року.

- Ендрю Келлі - головний розробник Zig, а проект обслуговується спільнотою учасників.
---
- Перш ніж був створений Zig, Ендрю Келлі працював над проектом LDC, компілятором мови D, і проект Zig був інспірований проектом LDC.
---
## Features
---
- Безпека

- Продуктивність

- Можливість відлагодження

- Кроскомпіляція

- Немає прихованого потоку управління

- Немає прихованого виділення пам'яті

- Немає макросів
---
- Немає файлів заголовків

- Немає залежностей

- Немає виконавчого середовища

- Немає сбірника сміття

- Немає невизначеної поведінки

- Немає глобального стану
---
## Code examples
---
### Hello, World!
```zig
const std = @import("std");

pub fn main() void {
    const stdout = std.io.getStdOut().writer();
    try stdout.print("Привіт, {}!\n", .{"світ"});
}
```
---
### Fibonacci sequence
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
## Memory management
---
- У Zig немає сбірника сміття та прихованих виділень пам'яті.

- Управління пам'яттю виконується вручну, і в Zig існує вбудований алокатор.

- У Zig існує вбудований алокатор, і можливо створювати власні алокатори.
---
## Code examples
---
### Виділення пам'яті
```zig
const std = @import("std");

pub fn main() void {
    const allocator = std.heap.page_allocator;
    const ptr = allocator.allocate(u8, 100);
    defer allocator.deallocate(ptr);
}
```
---
## Parallelism
---
## Zig has built-in support for:
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

async fn main() void { // асинхронна головна функція допускається в Zig
    await sleep(1000);
    const stdout = std.io.getStdOut().writer();
    try stdout.print("Привіт, світе!\n");
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
        try stdout.print("Привіт, світе!\n");
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
- Умовні змінні:

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
        try stdout.print("Привіт, світе!\n");
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
- Планування потоків:

```zig
const std = @import("std");

pub fn main() void {
    const thread = std.Thread.init(async {
        const stdout = std.io.getStdOut().writer();
        try stdout.print("Привіт, світе!\n");
    });
    defer thread.join();
}
```
---
- Управління потоками:

```zig
const std = @import("std");

pub fn main() void {
    const thread = std.Thread.init(async {
        const stdout = std.io.getStdOut().writer();
        try stdout.print("Привіт, світе!\n");
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
