
# Threads & JavaScript Concurrency

## What is a Thread?

A **thread** is the **smallest unit of execution** that the CPU can run.
It has its own **call stack**, **execution context**, and runs code **sequentially**.

**In short:**

> A thread is where your code runs.

---

## Single-Thread vs Multi-Thread

### Single-Thread

* One **thread** executes code.
* One **call stack**.
* No **race conditions** in user code.
* Uses **asynchronous I/O** instead of multiple threads.

**Best for:** I/O-bound tasks.

### Multi-Thread

* Multiple **threads** run in parallel.
* Multiple **call stacks**.
* Requires **synchronization** to avoid race conditions.
* Can fully use **multiple CPU cores**.

**Best for:** CPU-bound tasks.

---

## Why JavaScript is Single-Threaded

* Designed for **browser environments**.
* Prevents **DOM conflicts** (two threads modifying UI at the same time).
* Simpler **execution model** and **scope management**.

**In short:**

> JavaScript executes on **one main thread** to make UI updates safe and predictable.

---

## Event Loop (Key Concept)

Although JavaScript is **single-threaded**, it handles async work using:

* **Event Loop**
* **Task Queues**
* **Web APIs / libuv thread pool**

Async tasks run **outside** the main thread, and callbacks return to the **call stack** when ready.

**Model:**

> **Single thread** for JS execution + **thread pool** in the environment for async operations.

---

## Why Single-Thread is Not a Limitation

* Network I/O, timers, and file I/O do **not block** the main thread.
* The heavy work is done by **system threads**, while JS remains **non-blocking**.

This gives concurrency **without** dealing with thread synchronization.

