# How to contribute to open-source software?

This note is a running compilation of tips and tricks (recipe?) used
to efficiently contribute to open-source software.

## Recipe

### 1) Read the f\*\*\*\* documentation!

If the OSS project is mature enough, it should have a nice documentation. Otherwise, a good contribution might be to write a documentation for the project...

All things considered, the best way to start contributing to a project is to read the documentation to understand the structure of the project (e.g.: overloading built-in Numpy functions in Numba with `@overloading`).

### 2) Clone the repo and compile it

For rust crate, run a simple

```
cargo build
```

This ensures that you have the right dependencies installed to properly build and run a crate. It is important to check that things were working properly before you changing any parts of the code.

### 3) Run an example

Go to the example section of the documentation, and run a basic example. Another first good contribution for starting projects is to create starting examples, that are straight forward.

### 4) Start contributing

A useful resource: https://www.youtube.com/watch?v=8UzLuMiGs9s

## Best practices

### Write a precise and detailed commit history

When writing a PR message, explain your contributions in the following order:

1. What was being done before? How was the code organized before?

2. What does the PR change? What did you add?

3. Why is it a good thing?
