[参考链接](https://blog.csdn.net/weixin_36630761/article/details/78779221)

消息日志名是最小offset位置，消息在日志文件中的位置加上日志文件名的 offset 就是消息的 offset 位置（也就是 partiton 中的位置 offset ）,而系统每生成一个新的日志后会就将最后的offset作为新日志文件的文件名。我们可以认为kafka 消息日志文件里的offset实际就相当于是一个增量序列索引。 



offset 是每个分区内部唯一，不是全局唯一。