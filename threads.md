# Thread Launcher Utility

This utility provides a **helper function to create and start threads** in C++ with optional CPU core affinity and startup verification. It is useful in **high-performance, multithreaded applications** where threads need to be pinned to specific cores.

---

## Features

- Create threads for any callable (functions, lambdas, functors) with arguments.
- Optional **CPU core affinity** to bind a thread to a specific core.
- Ensures the thread has successfully started before returning.
- Graceful handling of thread startup failures.
- Returns a pointer to the running `std::thread` object.

---

## Usage

```cpp
auto worker = createAndStartThread(
    1,                    // CPU core to pin to (-1 for no affinity)
    "WorkerThread",       // Thread name (for logging)
    runTask,              // Callable function or lambda
    arg1, arg2            // Arguments to the callable
);
```

- `worker` is a pointer to the created thread.
- If core affinity setup fails, the function returns `nullptr`.
- Always `join` or `detach` the thread when done:

```cpp
if (worker) {
    worker->join();
    delete worker;
}
```

---

## Notes

- Requires a `setThreadCore(int core_id)` function that sets thread affinity.
- Uses **atomic flags** to synchronize main and child thread at startup.
- Designed for **Linux systems** (uses `pthread_self` and core affinity APIs).
