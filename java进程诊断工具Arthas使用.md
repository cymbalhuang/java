1.https://github.com/alibaba/arthas/releases下载包解压  
2.java -jar arthas-boot.jar运行  
3.填入要诊断java进程序号  
4.输入命令  
```
trace org.apache.hadoop.hbase.client.ClientScanner call '#cost>100' --skipJDKMethod false -n 1000
```
打印类ClientScanner中call函数耗时超过100毫秒的调用链，打印包含JDK函数调用，打印个数1000条  
5.使用完使用reset命令，重置增强类，将被 Arthas 增强过的类全部还原  
6.使用stop命令关闭Arthas服务端，所有 Arthas 客户端全部退出，stop时会重置所有增强过的类

官方文档  
https://arthas.aliyun.com/doc/index.html
