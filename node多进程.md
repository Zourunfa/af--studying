## node事件循环

**当nodejs启动时会初始化一个event loop,每一个event loop都会包含如下六个事件循环和浏览器的事件循环*完全不一样***

![image-20210520172620745](C:\Users\47302\AppData\Roaming\Typora\typora-user-images\image-20210520172620745.png)

1. **timers(定时器)**:此阶段执行那些由setTimeout和setInterval调度的回调函数
2. **I/O callback(I/O回调)** 此阶段几乎会执行所有回调函数，处理close callbacks和那些timers 与setImmediate调度的回调

> setImmediate约等于 setTimeout(callback,0)

1. idea(空转),prepare；此阶段只在内部使用
2. **poll(轮询)：**检查新的I/O事件；在查档的时候Node会阻塞在这个阶段
3. check(检查)： setImediate设置的回调会在这个阶段执行
4. close callback(关闭事件的回调):例如sockey.on('close',)此类回调再这个阶段执行