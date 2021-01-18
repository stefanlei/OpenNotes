##### Linux 进程状态

```sh
  PID STAT   TT  STAT      TIME COMMAND
46026 Ss+  s000  Ss+    0:00.61 /bin/zsh -l
47421 Ss+  s001  Ss+    0:00.21 /bin/zsh -l
48876 Ss   s002  Ss     0:00.66 /bin/zsh -l
50365 S+   s002  R+     0:01.38 ./test       # 在运行的主进程
50366 Z+   s002  Z+     0:00.00 (test)       # 已经僵死的子进程
50123 S    s003  S      0:00.29 -zsh
```



```cpp
R-- 运行状态（running）: 并不意味着进程一定在运行中，它表明进程要么是在运行中要么在运行队列里。
S-- 睡眠状态（sleeping): 意味着进程在等待事件完成（这里的睡眠有时候也叫做可中断睡眠（interruptible sleep)
D-- 磁盘休眠状态（Disk sleep）有时候也叫不可中断睡眠状态（uninterruptible sleep），在这个状态的进程通常会等待IO的结束.
T-- 停止状态（stopped）： 可以通过发送 SIGSTOP 信号给进程来停止（T）进程。这个被暂停的进程可以通过发送 SIGCONT 信号让进程继续运行。
X-- 死亡状态（dead）：这个状态只是一个返回状态，你不会在任务列表里看到这个状态。
Z-- 僵尸状态 (zombie) : 已经死了, 依然占着某些资源
```

```
< 高优先级
N 低优先级
L 有些页被锁进内存
s 包含子进程
+ 位于前台的进程组
l 多线程，克隆线程 multi
```

*如果代码中使用了 sleep ，那么在用 ps 查看的时候，一瞬间进程状态可能是 sleep*

