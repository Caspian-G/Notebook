#### <semaphore.h> Linux信号量

信号量可简单解决互斥和同步问题

**注** win信号量在<windows.h>  另外有<semaphore\>库支持跨平台但需c++20

##### 创建信号量

```
sem_t sem;
sem_init(&sem, 0, 0);
//参数1 指向信号对象 
//参数2 控制信号量类型 0则表示信号量是当前进程的局部信号量 否则信号量可在多个进程间共享
//参数3 信号量的初始值
//success返回0 failure返回-1
```

##### 信号量值+1  V操作

```
sem_post(&sem);
//success返回0 failure返回-1
```

##### 信号量值-1  P操作

```
sem_wait(&sem);
//success返回0 failure返回-1
```

##### 销毁信号量

```
sem_destroy(&sem);
//success返回0 failure返回-1
```

**注** 通过创建多个信号量就可实现线程的先后执行顺序

**注** 实际上mutex互斥锁也可实现同步 但更麻烦且需要通过忙等实现浪费cpu资源 而信号量和条件变量则是采用阻塞线程的策略非忙等
