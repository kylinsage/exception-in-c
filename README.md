# Exception in C

A simple exception handling framework for C language, using non-local jump functions `setjmp` and `longjmp`.

## Implementation

The pseudo keywords (`try`, `catch` and `finally`) are macros based on `setjmp` and `longjmp`. Whenever we start a `try ... catch ... finally` block, a non-local jump buffer is pushed at the top of a jump stack and is popped before exception handling in `catch` clause. So that, we can rethrow a caught exception. Otherwise, if the current jump buffer is not popped, any `longjmp` will cause infinite loop.

## Usage

### Catching Exceptions

We can catch exceptions (usually unsigned integers) with `try ... catch ... finally` blocks.

```
try {
    // code that may throw exceptions
} catch(exception1) {
    // code that processes the exception1
} catch(exception2) {
    // code that processes the exception2
} finally {
    // defautl exception handling
    // code that processes other exceptions
}
```

Note: we can fetch the exception code (an integer) by accessing variable `_except_code_` inside `try ... catch ... finally` blocks.

### Throwing Exceptions

We can throw an integer (non-zero) as an exception, for example:

```
throw(1);
```

## Ceveats

This framework provides basic exception handling with `setjmp` and `longjmp`, and is not a full-blown exception machansim like C++. So, there are a few ceveats and limitations:

* After invoking longjmp(), non-volatile-qualified local objects should not be accessed if their values could have changed since the invocation of setjmp(). Their value in this case is considered indeterminate, and accessing them is undefined behavior.
* Don't `return` directly in the `try ... catch ... finally` blocks.
* Memory allocated within `try ... catch ... finally` blocks is not automatically released when an exception is thrown.
* The semantics of `finally` clause is different from that in Java.
* A caught exception can be rethrown.
* The `try ... catch ... finally` blocks can be nested. It's recommended to use `{}` beside statements. Otherwise, using `_except_code_` will fail.

## References
* [Exceptions in C with Longjmp and Setjmp](http://www.di.unipi.it/~nids/docs/longjump_try_trow_catch.html)
* [CTurt/Exception](https://github.com/CTurt/Exception)
* [setjmp/longjmp and local variables](https://stackoverflow.com/questions/1393443/setjmp-longjmp-and-local-variables)
* [Performing Non-Local Goto](http://www.csl.mtu.edu/cs4411.ck/www/NOTES/non-local-goto/goto.html)
