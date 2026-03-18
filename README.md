# Thread Pool (C++)

## Overview

This project implements a **basic fixed-size thread pool** in C++ using standard library concurrency primitives. The thread pool allows tasks to be submitted from the main thread and executed concurrently by a pool of worker threads.

The implementation focuses on **correct synchronization, safe thread lifecycle management, and clean shutdown semantics**, demonstrating core multithreading concepts used in backend and systems programming.

---

## What This Project Does

* Creates a fixed number of worker threads at initialization
* Maintains a shared task queue
* Allows tasks (`std::function<void()>`) to be enqueued dynamically
* Worker threads wait efficiently for new tasks
* Ensures all threads terminate safely during destruction

---

## Key Components

### `ThreadPool`

* Manages worker threads and shared task queue
* Spawns threads in the constructor
* Joins all threads in the destructor

### Task Queue

* Implemented using `std::queue<std::function<void()>>`
* Protected by a `std::mutex`
* Coordinated using a `std::condition_variable`

### Worker Threads

* Each worker runs in a loop
* Waits until a task is available or shutdown is requested
* Executes tasks outside the critical section

---

## Concurrency Concepts Used

* `std::thread`
* `std::mutex`
* `std::lock_guard`
* `std::unique_lock`
* `std::condition_variable`
* `std::atomic<bool>`
* Producer–consumer pattern

---

## Thread Pool Lifecycle

1. **Initialization**
   Worker threads are created and begin waiting for tasks.

2. **Task Submission**
   Tasks are pushed into the shared queue and workers are notified.

3. **Task Execution**
   Available workers dequeue and execute tasks concurrently.

4. **Shutdown**
   On destruction, the stop flag is set, all workers are notified, and threads are joined safely.

---

## Example Usage

The `main.cpp` file demonstrates:

* Creating a thread pool with a fixed number of threads
* Submitting multiple tasks
* Concurrent execution across worker threads
* Graceful program termination after task completion

---

## Build Instructions

```bash
g++ -std=c++17 -pthread threadpool.cpp main.cpp -o threadpool
./threadpool
```

---

## Notes

* Tasks are executed in FIFO order
* The thread pool size is fixed at construction time
* Designed for clarity and correctness rather than advanced scheduling optimizations

## Author
Mohit Gunani
IIT BHU
