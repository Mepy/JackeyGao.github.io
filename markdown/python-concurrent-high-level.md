title: Python 高级并发
date: 2015-09-30 10:15:40
categories: 
- Python
- Python高级并发
tags:
- Python
- Concurrent

---

并发级别归纳为下列三种：

## 并发的归类

### 低级并发(Low-level Concurrency)


就是直接用『原子操作』(atomic operation)所实现的并发。这种并发是给程序库的编写者用的， 而应用程序开发者则不需要它，因为这种写法很容易出错，而且极难调试。虽说Python本身的并发机制一般是用底层的操作实现的， 但开发者不能用Python语言编写这种级别的并发代码。

### 中级并发(Mid-level Concurrency)

不直接使用原子操作， 但却会直接使用"锁"（lock）,大多数语言提供的都是这种级别的并发。Python的threading.Semaphore、threading.Lock及multiprocessing.Lock等类都支持中级并发。开发应用程序的人一般都会使用终极并发，因为他们通常只能使用这个级别的并发功能。

### 高级并发(High-level Concurrency)

既不直接使用原子操作， 也不直接使用锁(锁与原子操作可能在幕后使用， 但开发者无须关注这些。)目前已经有编程语言开始支持高级并发了。从3.2版本起，Python提供了支持高级并发的concurrent.futures模块，此外， queue.Queue及multiprocessing这两个『队列集合类』(queue collection class)也支持高级并发.

## 见解

### 中级并发痛点

中级并发是我经常使用的， 这种并发等级虽说使用起来相对低级并发简单， 但很容易出错， 容易出现那种难于追踪而且调试起来非常复杂。此外还会导致程序莫名的崩溃、失去响应(frozen)或者僵尸(zombie)进程.

### 关于中级并发建议

关键问题处在共享数据上面。如果共享数据可以修改，那么必须用锁来保护，以确保所有线程和进程都能按照顺序来存取它(也就是说你必须在程序里面控制好同一时刻必须有一个线程或进程访问这份数据)。如果有多个线程或进程试图访问同一份共享数据， 那么只有其中一个能够获取到， 而其他的都会阻塞(也就是进去『空闲状态』(idle)状态)。这就意味着当锁定机制生效时， 应用程序只有一个线程或进程起作用(这就变得和非并发程序类似了)，其余都得等待。由此可见我们应该进程少用锁，即便要用也不要时间太长,最简单的方法是根本不要分享可以修改的数据， 使进程本身只做运算，和I/O操作， 操作完后返回结果。这样就不用加锁了， 而大部分并发问题也就随之消失了.

### 高级并发

后面会追加一些高级并发， 对于Python来说高级并发才显得`Pythonic`
