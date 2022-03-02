# Rust smart pointers

Before diving into concurrency in Rust, we need to be comfortable with smart pointers.

## General overview

### What is a pointer?

A pointer is a general term to designate z variable that contains an address in memory.

The most basic pointer is the reference, which borrows the value they point to. **They don't have special capabilities**.

### What is a smart pointer?

A smart pointer has additional capability compared to mere reference. They have additional metadata and capabilities.

Another major difference in Rust is that smart pointers usually own the data they point to.

```
Remember that ownership is the key feature that allows Rust not to have a garbage collector and to be memory-safe. The three rules of ownership are:

- Each value has a variable that's called its owner.
- There can be only one owner at a time.
- When the owner goes out of scope, the value will be dropped.
```

### How is implemented a smart pointer?

A smart pointer is essentially a `Struct` data structure that implements the `Deref` and `Drop` traits.

The `Deref` traits allows an instance of a smart pointer struct to behave like a reference so we can write code that works with either references of smart pointers or actual smart pointers.

The `Drop` trait specifies a custom logic when an instance of a smart pointer goes out of scope.

## `Box<T>` to point to data on the heap

### What is the purpose of this smart pointer?

Boxes allows to move data to the heap while keeping a pointer to that data on the stack.

3 main use cases:

- Wrap a type whose size is not known at compile time and you want to use a value of that type in a context that requires an exact size.

A canonical example:

```
struct Node<T> {
    value: T,
    next: Node<T>
}
```

This code does not compile in Rust, since the recursive structure of `Node` prevents the compiler to infer the exact size of the structure. To circumvent this issue, we need `Box`:

```
struct Node<T> {
    value: T,
    next: Box<Node<T>>
}
```

- When you have a large amount of data and you want to transfer ownership, but you want to make sure data is not copied.

The idea here is that by keeping the actual data on the heap, transferring ownership from one variable to another will actually comes down to copying a small amount of pointer data around on the stack, not the actual data on the heap.

- Use it as trait object.

More on this later.
