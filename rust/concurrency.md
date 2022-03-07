# Concurrency in Rust

Within a process, independent parts of a program can be run in **threads** (fil d'exécution).

**Race conditions**: threads are accessing data or resources in an inconsistent order.

**Deadlocks**: two threads are waiting for each other to finish using a resource the other thread has, preventing both threads from continuing.

**Green threads**: virtual threads created by the programming language using its own API. Not to be confounded with operating system threads, which are created by the OS, not the programming language.

By default, Rust does not support green threads, but crates like `Tokio` does implement green threads.

## Creating a new thread

### `thread::spawn`

`thread::spawn` is called with a closure to run a new thread.

```
use std::thread;
use std::time::Duration;

fn main() {
    thread::spawn(|| {
        for i in 0..10 {
            println!("hi number {}", i);
            thread::sleep(Duration::from_millis(1));
        }
    });
}
```

A `thread::spawn` call returns an owned value of type `JoinHandle`. `JoinHandle` can call a method `join` which waits for the spawned thread to finish executing.

### Using `move` closures with threads

`move` used with closure allows the closure to capture the variables it's using as part of the closure's environment. See this example:

```
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(|| {
        println!("Here's a vector: {:?}", v);
    });

    drop(v); // oh no!

    handle.join().unwrap();
}
```

This code can't compile since there's a possibility the code in the closure in the spawned thread is executed after `v` is dropped, in which case `v` can't be printed. We need to give ownership of the values `v` to the closure, in order for the spawned thread to use it. Therefore, we also need to remove the `drop(v)` line since `v` is no longer accessible from the main thread.

```
use std::thread;

fn main() {
    let v = vec![1, 2, 3];

    let handle = thread::spawn(move || {
        println!("Here's a vector: {:?}", v);
    });

    handle.join().unwrap();
}
```

## Using message passing to transfer data between threads

To ensure safe concurrency, we rely among others on **message passing**, where threads or actors communicate by sending each other messages containing data.

In Rust, message passing is implemented using **channels**. Channels take a **transmitter** and a **receiver** and makes information pass within the channel. A channel is said to be **closed** when either the transmitter of the receiver half is dropped.

`std::sync::mpsc` stands for _Multiple producer, single consume_. It is a kind of channel where multiple producers can send a message but only one receiver will consume the message.

```
use std::sync::mpsc;
use std::thread;

fn main() {
    let (tx, rx) = mpsc::channel();

    thread::spawn(move || {
        let val = String::from("hi");
        tx.send(val).unwrap();
    });

    let received = rx.recv().unwrap();
    println!("Got: {}", received);
}
```

Note that for the receiver ends, two methods exist to parse a message `recv` and `try_recv`. `try_recv` is a non-blocking function that immediately returns `Result<T, E>`.

To create multiple transmitters, we just have to clone the transmitter:

```
let (tx, rx) = mpsc::channel();
let tx1 = tx.clone();
```

## Shared-state concurrency

Shared-state concurrency consists in allowing different threads to access a variable in memory. This is a difficult
task since it essentially comes down to multiple threads having ownership on one single value.

### Using mutexes to allow access to data from one thread at a time

**Mutex**: mutual exclusion.

A mutex allows only one thread to access some data at any given time. A locking mechanism signals that one
thread wants to access the data, and prevents the other from accessing it until it is unlocked. It **guards**
the data.

Two rules:

- Lock the data before accessing it
- Unlock it after you changed it

```
use std::sync::Mutex;

fn main() {
    let m = Mutex::new(5);

    {
        // Block the current thread with lock()
        // unwrap() is used to make the program panic if another thread has locked
        // the mutex
        let mut num = m.lock().unwrap();
        *num = 6;
    }

    println!("m = {:?}", m);
}
```

The call to `lock()` returns a smart pointer called **MutexGuard**. It implements `Deref`, which enables us
to point at our inner data. It also implements `Drop` which automatically releases the lock when the smart
pointer goes out of scope.

### Atomic reference counting with `Arc<T>`

When trying to pass a `Mutex` to different threads, multiple threads want to have access to the data, so
to create multiple references, we use the smart pointer `Arc`. Note that `Rc` **does not work** as it is
not thread-safe and can introduce memory leaks.

Arc: atomically reference counted. **Atomic** are a kind of primitive types implemented in `std::sync::atomic`
that provide primitive shared-memory communication between threads.

Note that atomic types comes at a performance penalty, and thus should be used when really needed.

The `Send` trait indicates that ownership of values of type implementing `Send` can be transferred between threads.
