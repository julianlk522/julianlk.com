+++
title = 'General-Purpose Programming'
date = 2024-11-21T19:29:54-05:00
layout = 'qna-section'
draft = false
+++

## What is the point of closures? And why not combine the inner and outer functions?

Closures allow creating hidden state inside the outer function's lexical scope (the _enclosing_ scope) which is preserved (_captured_) after the outer function finishes executing. The hidden state is accessible (i.e., can be referenced or _closed over_) in the inner function but safely separated from the rest of your code. And each call to the outer function spawns a new, distinct instance of the encapsulated state.

```
function create_counter() {
    let count = 0

    // Inner function (closure)
    function counter() {
        console.log(++count)
        return count
  }

    return counter
}
```

Distinct instances of state encapsulation for each call:

```
const counter1 = create_counter()
const counter2 = create_counter()

counter1() // 1
counter1() // 2
counter1() // 3

counter2() // 1
counter2() // 2
```

State is "safe," not directly accessible:

`console.log(counter1.count) // undefined`

A function containing no other functions _could_ contain its own state, isolated from the rest of the program, but the difference is that it cannot be invoked `n` times to produce `n` autonomous instances; it is limited to the single instance of state declared in its definition, which also cannot persist between calls.

Attempting to merge outer and inner functions...

```
function counter() {
    let count = 0
    console.log(++count)
    return count
}
```

...prevents isolated instances with preserved state:

```
const counter1 = () => counter()
const counter2 = () => counter()

counter1() // 1
counter1() // 1
counter2() // 1
counter2() // 1
```

You could store the state outside of function scope, which would allow it to persist during execution of the program, but that would also expose it to outside manipulation and require knowing in advance how many instances of that state need to exist.

So, closures are a way to allow dynamic regeneratation of private, preserved state.

The same effect can be achieved with a class. The private state could be represented as a property of the class and the inner function could be a method. Calling a class constuctor (e.g., `new Thing(...)`) would be equivalent to calling the outer function.

Both the [OOP](https://en.wikipedia.org/wiki/Object-oriented_programming) implementation (classes) and [FP](https://en.wikipedia.org/wiki/Functional_programming) implementation (closures) are valid, depending on the programming language and existing design patterns in the codebase, though classes do not always guarantee separation of state from other parts of the codebase.

Example with classes:

```
class Counter {
  constructor() {
    this._count = 0
  }

  increment() {
    console.log(++this._count)
    return this._count
  }
}

const counter1 = new Counter()
const counter2 = new Counter()

counter1.increment() // 1
counter1.increment() // 2
counter1.increment() // 3

counter2.increment() // 1
counter2.increment() // 2
```

Property is _not_ private in JavaScript classes:

`console.log(counter1._count) // 3`

## What is the point of generator functions? When are they preferable to ordinary functions?

**TL;DR**:
they aren't explicitly required for anything, but they may help with organization and readability.

1.  Breaking up resource requirements for expensive tasks (i.e., [_lazy evaluation_](https://www.tutorialspoint.com/functional_programming/functional_programming_lazy_evaluation.htm)). Maybe you want to read lines from a 4GB file without loading the entire 4GB into memory, or read data coming from an infinite stream, so you create a generator function to stream line by line. Unused lines are never consumed and their memory requirements can be avoided.

    You'll need to keep track of how much of the file / stream has already been read as you continue. Managing state between calls like this is possible with closures but generators do it inherently.

With closure:

```
function lazy_sequence() {
    let i = 0;
    return function() {
        return i++
    }
}

const sequence = lazy_sequence()
console.log(sequence()) // 0
console.log(sequence()) // 1
console.log(sequence()) // 2
```

With generator:

```
function* lazy_sequence() {
    let i = 0;
    while (true) {
        yield i++
    }
}

const sequence = lazy_sequence()
console.log(sequence.next().value) // 0
console.log(sequence.next().value) // 1
console.log(sequence.next().value) // 2
```

2. Generators also integrate nicely into [iterators](https://en.wikipedia.org/wiki/Iterator) and allow usage of `for-of` loops and other constructs that can simplify readability.

```
async function asynchronous_task() {
    await new Promise(...)
    ...
}
```

With callback:

```
async function stream_task_results(callback, stop_condition) {
    while (true) {
        const task_results = await asynchronous_task()
        await callback(task_results)
        if (stop_condition(task_results)) {
            break
        }
    }
}

stream_task_results(
    task_results => do_something(task_results), // callback
    task_results => is_time_to_stop(task_results) // stop_condition
)
```

With generator:

```
async function* stream_task_results() {
    while (true) {
        yield await asynchronous_task()
    }
}

async function consume_stream() {
    const task_results = stream_task_results()
    for await (const tr of task_results) { // convenient iterator syntax
        do_something(tr)
        if (is_time_to_stop(tr)) {
            break
        }
    }
}

consume_stream()
```

## Are there _exclusive_ use cases for classes compared to functions? Is there a strong reason to use them, aside from just preference or matching some existing code's design themes?

1. Classes provide distinct namespaces, functions do not
    - no mixing up `foo()` and `foo()` if they are `bar.foo()` and `baz.foo()`
2. (opinion) Classes intuitively group related data and separate unrelated data
    - see [https://www.reddit.com/r/learnpython/comments/1mc8ih/comment/cc7uyxx](https://www.reddit.com/r/learnpython/comments/1mc8ih/comment/cc7uyxx)
