# 5.2 Concurrency Patterns

## Table of Contents
- [Introduction to Concurrency Patterns](#introduction-to-concurrency-patterns)
- [Synchronization Patterns](#synchronization-patterns)
  - [Monitor Pattern](#monitor-pattern)
  - [Producer-Consumer Pattern](#producer-consumer-pattern)
  - [Reader-Writer Lock Pattern](#reader-writer-lock-pattern)
  - [Thread Pool Pattern](#thread-pool-pattern)
- [Asynchronous Patterns](#asynchronous-patterns)
  - [Future/Promise Pattern](#futurepromise-pattern)
  - [Actor Model Pattern](#actor-model-pattern)
  - [Reactor Pattern](#reactor-pattern)
- [Advanced Examples and Case Studies](#advanced-examples-and-case-studies)
- [Review Questions](#review-questions)
- [Answers and Explanations](#answers-and-explanations)
- [Further Reading and Multimedia Resources](#further-reading-and-multimedia-resources)

---

## Introduction to Concurrency Patterns

Concurrency patterns address the challenges of managing multiple tasks executing simultaneously or in parallel. They help ensure correctness, performance, and scalability in multi-threaded and distributed systems.

**Key Concepts:**
- Thread safety, synchronization, and deadlock avoidance
- Task scheduling and resource sharing
- Asynchronous programming and event-driven architectures

**Advanced Perspective:**
- Concurrency is essential for high-performance, scalable systems (e.g., web servers, data processing pipelines).
- Modern languages (Python, Java, Go) provide built-in concurrency primitives and libraries.

**Critical Thinking Prompt:**
How do concurrency patterns help prevent race conditions and deadlocks in real-world systems?

---

## Synchronization Patterns

### Monitor Pattern
- **Intent:** Synchronize method execution to ensure only one thread accesses critical sections at a time.
- **What it achieves:**
  - Prevents race conditions and data corruption
  - Simplifies thread-safe class design
- **Python Example:**
```python
import threading
class Counter:
    def __init__(self):
        self.value = 0
        self.lock = threading.Lock()
    def increment(self):
        with self.lock:
            self.value += 1
```

### Producer-Consumer Pattern
- **Intent:** Decouple producers (generating data) from consumers (processing data) using a shared buffer or queue.
- **What it achieves:**
  - Balances workload between producers and consumers
  - Supports asynchronous processing and buffering
- **Python Example:**
```python
import threading, queue
q = queue.Queue()
def producer():
    for i in range(5):
        q.put(i)
def consumer():
    while True:
        item = q.get()
        print(f"Consumed: {item}")
        q.task_done()
```

### Reader-Writer Lock Pattern
- **Intent:** Allow multiple readers or a single writer to access a resource, improving concurrency for read-heavy workloads.
- **What it achieves:**
  - Increases throughput for read-dominated systems
  - Prevents data inconsistency during writes
- **Python Example:**
```python
import threading
class RWLock:
    def __init__(self):
        self.lock = threading.Lock()
        self.readers = 0
    def acquire_read(self):
        with self.lock:
            self.readers += 1
    def release_read(self):
        with self.lock:
            self.readers -= 1
    def acquire_write(self):
        self.lock.acquire()
    def release_write(self):
        self.lock.release()
```

### Thread Pool Pattern
- **Intent:** Reuse a pool of threads to execute tasks, reducing overhead of thread creation and destruction.
- **What it achieves:**
  - Improves performance and resource utilization
  - Simplifies task scheduling and management
- **Python Example:**
```python
from concurrent.futures import ThreadPoolExecutor
def task(n):
    print(f"Processing {n}")
with ThreadPoolExecutor(max_workers=4) as executor:
    for i in range(8):
        executor.submit(task, i)
```

---

## Asynchronous Patterns

### Future/Promise Pattern
- **Intent:** Represent a value that may become available in the future, enabling asynchronous computation and result retrieval.
- **What it achieves:**
  - Decouples task submission from result consumption
  - Supports non-blocking programming
- **Python Example:**
```python
from concurrent.futures import Future
def compute():
    return 42
future = Future()
future.set_result(compute())
print(future.result())
```

### Actor Model Pattern
- **Intent:** Structure concurrent systems as independent actors that communicate via message passing.
- **What it achieves:**
  - Avoids shared state and locks
  - Supports scalability and fault tolerance
- **Python Example:**
```python
import queue, threading
class Actor(threading.Thread):
    def __init__(self):
        super().__init__()
        self.inbox = queue.Queue()
    def send(self, msg):
        self.inbox.put(msg)
    def run(self):
        while True:
            msg = self.inbox.get()
            print(f"Received: {msg}")
```

### Reactor Pattern
- **Intent:** Handle service requests delivered concurrently to an event handler, using a single-threaded event loop.
- **What it achieves:**
  - Efficiently manages I/O-bound tasks
  - Supports high concurrency with minimal threads
- **Python Example:**
```python
import selectors, socket
sel = selectors.DefaultSelector()
# ...register sockets and handle events...
```

---

## Advanced Examples and Case Studies

### Case Study 1: High-Performance Web Server
- **Scenario:** A web server must handle thousands of simultaneous connections.
- **Solution:**
  - Use Reactor pattern for event-driven I/O
  - Employ Thread Pool for CPU-bound tasks
- **Outcome:** High throughput, low latency, and efficient resource usage.

### Case Study 2: Real-Time Analytics Pipeline
- **Scenario:** A data analytics system ingests and processes streaming data.
- **Solution:**
  - Use Producer-Consumer for decoupling ingestion and processing
  - Apply Future/Promise for asynchronous result handling
- **Outcome:** Scalable, resilient, and responsive analytics platform.

---

## Review Questions
1. What is the difference between Monitor and Reader-Writer Lock patterns?
2. How does the Producer-Consumer pattern improve system scalability?
3. When should you use a Thread Pool instead of creating threads on demand?
4. What are the benefits of the Actor Model over shared-memory concurrency?
5. How does the Reactor pattern enable high concurrency with minimal threads?
6. Explain the role of Future/Promise in asynchronous programming.
7. Give a real-world example of combining multiple concurrency patterns.
8. What are the risks of improper synchronization in multi-threaded systems?
9. How do concurrency patterns relate to distributed systems?
10. Discuss the trade-offs between event-driven and thread-based concurrency.

---

## Answers and Explanations
1. Monitor synchronizes all access; Reader-Writer Lock allows concurrent reads but exclusive writes.
2. Producer-Consumer decouples workload, enabling parallelism and buffering.
3. Thread Pools reduce overhead and improve resource management for many short-lived tasks.
4. Actor Model avoids shared state, reducing bugs and improving scalability.
5. Reactor uses event loops to handle many connections with few threads.
6. Future/Promise enables non-blocking result retrieval and task coordination.
7. Web server: Reactor for I/O, Thread Pool for processing, Future for async responses.
8. Risks: race conditions, deadlocks, data corruption.
9. Concurrency patterns underpin distributed systems (e.g., Actor Model in Akka, Erlang).
10. Event-driven is efficient for I/O-bound; thread-based is better for CPU-bound or blocking tasks.

---

## Further Reading and Multimedia Resources
- [Patterns for Parallel Programming (Timothy Mattson)](https://www.oreilly.com/library/view/patterns-for-parallel/0321228111/)
- [Python Concurrency Patterns (Real Python)](https://realpython.com/python-concurrency/)
- [YouTube: "Concurrency Patterns" (Academind)](https://www.youtube.com/watch?v=Qkz_Cb2p1aA)
- [YouTube: "Actor Model Explained" (CodeOpinion)](https://www.youtube.com/watch?v=KpF7pAq3y6E)
- [Microsoft Docs: Asynchronous Programming Patterns](https://docs.microsoft.com/en-us/dotnet/standard/asynchronous-programming-patterns/)

---

*End of 5.2 Concurrency Patterns*
