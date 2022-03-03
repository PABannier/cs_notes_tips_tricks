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

Because `Box<Node<T>>` is a pointer, Rust always know how much space `Box<Node<T>>` will take. It does not vary based on the amount of data it's pointing to. Now the compiler know that the size of an instance of `Node<T>` is the sum of an instance of type `T` and of a box's pointer data. By using a box, we've broken the infinite, recursive chain.

- When you have a large amount of data and you want to transfer ownership, but you want to make sure data is not copied.

The idea here is that by keeping the actual data on the heap, transferring ownership from one variable to another will actually comes down to copying a small amount of pointer data around on the stack, not the actual data on the heap.

- Use it as trait object.

More on this later.

### Creation and destruction of `Box`

Creation:

```
fn main() {
    let a = Box::new(5);
    println!("a = {}", a);
}
```

When `a` goes out of scope, the memory on the heap (which stores the actual data) and on the stack (which stores the pointer) will be
de-allocated.

## Treating smart pointers like regular references with the `Deref` trait

The `Deref` trait allows us to use the `*` operator. This operator is used with references:

```
let x = 5;
let y = &x;

assert_eq!(x, 5);
assert_eq!(*y, 5);
```

Much the same way, we can write

```
let x = 5;
let y = Box::new(x);

assert_eq!(x, 5);
assert_eq!(*y, 5);
```

The only difference between these two snippets of code is that in the second case `y` is an instance of a box pointing to a **copied** value of `x`, rather than a reference pointing to the value of `x`.

The `Deref` trait allows us to implement the `deref` method with the following signature:

```
pub trait Deref {
    type Target: ?Sized;
    fn deref(&self) -> &Self::Target;
}
```

By implementing the `Deref` trait, `Box` actually returns a reference to `Target` which can then be dereferenced by Rust, since Rust already knows how to dereference a `&` pointer. Strictly speaking, when dereferencing, this happens:

```
*(y.deref())
```

## Running code on cleanup with the `Drop` trait

The `Drop` traits lets you customize what happens when a value is about to go out of scope. For example, when `Box<T>` is dropped, it'll deallocate the space on the heap the box points to. This is a powerful trait, since in other languages like C++ smart pointers need to be explicitly dropped in order for memory to be deallocated. Here, Rust automatically handles the cleanup code when a variable is going out of scope.

To manually delete a value, call `std::mem::drop`.

## `Rc<T>`, the reference counted smart pointer

Most of the time, ownership is clear: one variable has ownership over one value. However, it happens that we need multiple variables to have ownership over one specific value.

For instance, consider a doubly linked list. A node has a previous and a next pointer. But a node has also a previous node pointing forward to it and a next node pointing in a backward fashion to it. Conceptually, that node is owned both by its previous and next neighbors. We need some smart pointers to allow multiple ownership.

`Rc<T>` is a smart pointer (short for _reference counting_) that keeps track the number of references to a specific value. When no reference is pointing to that value, the value can be cleaned up without any references becoming invalid.

Note that `Rc<T>` is used in **single-threaded** scenarios.

To increase the reference count, we must clone the smart pointer. Idiomatically, we prefer to use `Rc::clone` instead of `clone` method, since `clone` method is often used to perform deep copy (copy data on the heap), while cloning a smart pointer performs a shallow copy (copy the pointers pointing to the data, that are on the stack).
