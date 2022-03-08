# Tokio - An asynchronous Rust runtime

It provides the building blocks to write networking applications, from large servers
to small embedded devices.

3 major components:

- A multi-threaded runtime for executing asynchronous code
- An asynchronous version of the standard library
- A large ecosystem of libraries

## Getting started with Tokio

### `await` and `async`

In the function definition, `async` makes the function operate asynchronously. Any
calls to `.await` within the `async fn` yield control back to the thread. The thread
may do other work while the operation processes in the background.

```
async fn say_world() {
    println!("world");
}

#[tokio::main]
async fn main() {
    // Calling say_world() does not execute the body of `save_world()`
    let op = say_world();

    // This println! is executed first
    println!("Hello");

    // Calling `.await` on `op` starts executing `say_world`.
    op.await;
}
```

The return value of an `async fn` is an anonymous type that implements the `Future` trait.

### Async `main` function

Asynchronous functions are run in a runtime, which provides an asynchronous task scheduler,
provides evented I/O, timers, etc. The runtime does not automatically start and is started
by the main function, hence the `#[tokio::main]` macro.

## Spawning

### Accepting sockets

**A socket is one endpoint of a two-way communication link between two programs running on the network**. A socket is bound to **a port number** so that the TCP layer can identify the application that data is destined to be sent to.

**An endpoint: an IP address + a port number**.

To accept inbound TCP sockets, we need a TCPListener. There exists a `std::net::TCPListener`, which
is the synchronous version of `tokio::net::TCPListener`.

```
use tokio::net::{TcpListener};

#[tokio::main]
async fn main() {
    // Bind the listener to the address
    let listener = TcpListener::bind("127.0.0.1:6379").await.unwrap();

    loop {
        // The second item contains the IP and port of the new connection.
        let (socket, _) = listener.accept().await.unwrap();
        process(socket).await;
    }
}
```

### Concurrency

The limitation of the code above is that it can only handle one request at a time.
When a connection is accepted, the server stays inside the loop until the response
is fully written to the socket.

We need to add **concurrency** in order for the server to handle many requests **concurrently**.

**Concurrency**: one thread (or multiple threads) alternates between tasks.
**Parallelism**: multiple threads are performing the same task.

To add concurrency, we need to modify the code as follows:

```
use tokio::net::TcpListener:

#[tokio::main]
async fn main() {
    let listener = TcpListener::bind("127.0.0.1::6379").await.unwrap();

    loop {
        let (socket, _) = listener.accept().await.unwrap();
        // A new task is spawned for each inbound socket. The socket is moved
        // to the new task and processed there.
        tokio::spawn(async move {
            process(socket).await;
        });
    }
}
```

A Tokio `task` is an **asynchronous green thread**.
**Green thread**: A virtual thread that is scheduled by a runtime or a virtual machine, instead of natively by the underlying OS. **Green threads** enable **multi-threaded** environments without relying on any native OS abilities. They are managed by the user space (the programmer) instead of the kernel space.

The `tokio::spawn` function returns a `JoinHandle`, which the caller may use to interact with the spawned task. If the asynchronous function has a return value, the caller can access the return value by using `.await` on the `JoinHandle`.

```
#[tokio::main]
async fn main() {
    let handle = tokio::spawn(async {
        // Do some async work
        "return value"
    });

    // Do some other work

    let out = handle.await.unwrap();
    println!("GOT {}", out);
}
```

Spawning a task submits it to the `Tokio` scheduler.

Remember that data used within a task must be **moved** with the `move` keyword
in the async block. In other words, all variables in a task spawning block must
have a `'static` lifetime.

### The `Send` bound

All variable types in a spawned `async` block must implement the `Send` trait. The reason
is that variables can be moved in the background by Tokio from one thread to another by
the scheduler. To allow that, the types need to implement `Send`.
