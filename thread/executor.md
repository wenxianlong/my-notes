# Java并发——线程池Executor框架

## 线程池

### 无限制的创建线程池
若采用"为每个任务分配一个线程"的方式会存在一些缺陷,尤其是当需要创建大量线程时:
* 线程生命周期的开销非常高
* 资源消耗
* 稳定性

### 引入线程池
任务是一组逻辑工作单元,线程则是使任务异步执行的机制.当存在大量并发任务时,创建销毁线程需要很大的开销,运用线程池可以大大减少开销

## Executor框架
![](../image/executor.png)
