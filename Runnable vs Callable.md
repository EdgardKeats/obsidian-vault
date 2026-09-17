
In Java, **`Runnable` and `Callable` are both functional interfaces used to encapsulate a task meant to be executed concurrently by another thread**, but ==they differ fundamentally in how they handle return values and exceptions==. 

Quick Comparison

| Feature                | `Runnable`                                       | `Callable<V>`                                                |
| ---------------------- | ------------------------------------------------ | ------------------------------------------------------------ |
| **Introduced In**      | JDK 1.0                                          | JDK 1.5 (`java.util.concurrent`)                             |
| **Execution Method**   | `void run()`                                     | `V call()`                                                   |
| **Return Value**       | **Does not return a result** (`void`)            | **Returns a result** of generic type `V`                     |
| **Exception Handling** | Cannot throw checked exceptions                  | **Can throw checked exceptions**                             |
| **Thread Creation**    | Can be passed directly to `new Thread(runnable)` | Cannot be passed to `Thread`. Requires an `ExecutorService`. |

---

Key Differences Explained

1. Return Value

- **`Runnable`**: The `run()` method has a `void` return type. It is a "fire-and-forget" task. If you need a result back, you must rely on modifying shared memory or updating a database externally. 

- **`Callable`**: The `call()` method returns a generic value (`V`). When you submit a `Callable` to an `ExecutorService`, it returns a `Future<V>` object. You can later call `future.get()` to retrieve the completed calculation.

2. Exception Handling

- **`Runnable`**: You cannot throw checked exceptions from the `run()` method. Any checked exception inside the code must be caught and handled locally via a `try-catch` block. 

- **`Callable`**: The signature of `call()` includes `throws Exception`. This allows you to propagate checked exceptions up to the caller, which will then bubble up when you try to harvest the result using `Future.get()`.

3. Invocation and Mechanics

- **`Runnable`**: Extremely versatile. It can be run by passing it directly to a standard `Thread` instance, or by passing it to an `ExecutorService` via `.execute()` or `.submit()`.

- **`Callable`**: Cannot be used to instantiate a bare `Thread` object. It must be managed and executed through a thread pool via an `ExecutorService.submit()` call. 