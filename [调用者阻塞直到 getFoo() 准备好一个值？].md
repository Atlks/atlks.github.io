# [调用者阻塞直到 getFoo() 准备好一个值？] 



`wait()`而且`notifyAll()`我有 95% 的机会做对  一般来说 notify() 和 notifyAll() 是你想要的方法

[`CountDownLatch`](http://java.sun.com/javase/6/docs/api/java/util/concurrent/CountDownLatch.html)则 a 非常适合。

使用 中的任何一个[`BlockingQueue`](http://java.sun.com/j2se/1.5.0/docs/api/java/util/concurrent/BlockingQueue.html)更`java.util.concurrent`具体地说

您可以使用 wait() 和 notify() 方法：