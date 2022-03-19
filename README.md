### Hi there 👋

<!--
**Tomasmule2015/Tomasmule2015** is a ✨ _special_ ✨ repository because its `README.md` (this file) appears on your GitHub profile.

Here are some ideas to get you started:

- 🔭 I’m currently working on ...
- 🌱 I’m currently learning ...
- 👯 I’m looking to collaborate on ...
- 🤔 I’m looking for help with ...
- 💬 Ask me about ...
- 📫 How to reach me: ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...
插入代码块
插入行内代码，即插入一个单词或者一句代码的情况，使用 `code` 这样的形式插入。
插入多行代码，分别使用三个反引号（```）包裹多行代码。或者使用缩进 ```c++

2^8^=256
x<sup>2</sup>2
指数

下标
a~0~=1
a<sub>0</sub>

-->

## 浅谈时间复杂度
###时间复杂度
    时间复杂度在我之前刷leetcode的时候经常不考虑，以至于有时候解出的题目超时了，要想优化程序，时间复杂度的计算是必不可少的。
### 最优、最坏和期望情况
  最优： 就是输入的数据恰巧使得程序执行指令最少次数  
  最坏： 就是输入的数据恰巧使得程序执行指令最多次数  
  期望： 平均复杂度  
  
### 复杂度处理方式
删除常量： 大O仅仅描述了增长的趋势  
丢弃不重要的项：在乎的是更大的  
多项式算法： 如何区分加与乘？ 加： “做这个，结束之后做那个”  乘： "对这个的每个元素做那个”

### 各种排序算法的时间复杂度
|算法|平均复杂度|最优|最坏|空间复杂度|稳定性|
|---|----|----|----|----|----|
|冒泡排序| On<sup>2</sup>|
|---|----|----|----|----|----|
|选择排序| On<sup>2</sup>|
|---|----|----|----|----|----|
|插入排序| On<sup>2</sup>|
|---|----|----|----|----|----|
|快速排序|nlongn|
|---|----|----|----|----|----|
|堆排序| nlogn|
|---|----|----|----|----|----|
|希尔排序| nlogn|
|---|----|----|----|----|----|
|归并排序|nlogn|
|---|----|----|----|----|----|
|计数排序| n + k|
|---|----|----|----|----|----|
|基数排序| N * M|
|---|----|----|----|----|----|
### 递归
```c++
int sum(Node node) {
  if(node == NULL) {
    return 0;
  }
  return sum(node.left) + node.value + sum(node.right);
}
```
时间复杂度分析： 实际上是遍历了一遍所有节点，对每个节点访问一次，所以是O(n), n是节点数量。

```c++
int fib(int n) {
  if(n <= 0) return 0;
  else if (n == 1) return 1;
  return fib(n - 1） + fib(n - 2);
}
```
 
O(branches<sup>dep</sup>)
每个调用有两个分支，所以是O(2<sup>N</sup>)

打印所有从0到n的斐波那契数列，时间复杂度？
```c++
int fib(int n) {
  if(n <= 0) return 0;
  else if (n == 1) return 1;
  return fib(n - 1） + fib(n - 2);
}

void allfib(int n) {
  for (int i = 0; i < n; i++) {
    cout<<fib(i)<<" ";
  }
}
```
并不是n*2<sup>n</sup>
一个个推倒：
fib(1) = 2<sup>0</sup>
fib(2) = 2<sup>1</sup>
fib(3) = 2<sup>2</sup>
fib(4) = 2<sup>3</sup>
...
fib(n) = 2<sup>n - 1</sup>

累加： 2<sup>n</sup>

已经计算过的不再重复计算，那么时间复杂度是多少
实际上每个数值访问一次，O(n)

### 复杂排序
遍历字符串数组，取出每个字符串并对其排序，最后排序整个数组，时间复杂度是多少？

分析： 字符串最大长度为M， 数组长度为N
排序字符串需要 MlogM, 使用快排， 有N个字符串，因此需要N * MlogM
排序整个数组时，这里每比较一次并不是O(1), 因为比较字符串是需要比较每个字符串中的字符的，因此每比较一次是O(M)
所以单说数组排序就是 O(NlongN) * O(M)
再加上之前字符串排序用的时间， 则为 O（N*MlogM） * O(M*NlogN) = O(M*N(logM + logN));


# 操作系统  
### 锁的种类
互斥锁  
确保同一时间只有一个线程访问数据，
pthread_mutex_lock\pthread_mutex_trylock\pthread_mutex_unlock
trylock EBUSY 加锁失败

pthread_mutex_timelock 允许线程阻塞时间，是绝对时间。超过时间则不再加锁。
读写锁  
可以有多个线程同时读，但是只能有一个线程写。且在写模式下不能读。 避免读模式长期占用，可以在线程试图以写模式获取锁时，读写锁通常会阻塞随后的读模式锁请求。  
自旋锁  
自旋锁与互斥量类似，但是它不是通过休眠使进程阻塞，而是在获取锁之前一直处于忙等阻塞状态。 场景： 锁被持有的时间短，线程并不希望在重新调度上花费太多成本。  

屏障  
屏障是用户协调多个线程并行工作的同步机制。 屏障允许每个线程等待，知道所有的合作线程都到达某一点，然后从改点继续执行。  
允许任意数量的线程等待，直到所有的线程完成处理工作
eg： 8个线程对800w个数据进行排序，最后merge， 使用屏障，在满足屏障计数后，merge  

### 进程与线程
进程与线程的区别？进程间通信方式？
https://cloud.tencent.com/developer/article/1688297  
https://www.cnblogs.com/Survivalist/p/11527949.html#%E5%8D%8F%E7%A8%8B%E7%9A%84%E7%9B%AE%E7%9A%84 


线程是如何调度的？
抢占式调度，时间片 
先到先处理  
短任务优先  
优先级  
抢占  
多级队列  

### 3月17日
### C++11：并行与并发
std::thread用于创建一个执行的线程实例 get_id() join()  
```c++
#include <iostream>
#include <thread>
int main()
{
    std::thread t ([]() {
        std::cout<<" hello word."<<std::endl;
        });
        t.join();
     return 0;
}
```
或者thread t(func, args)进行初始化

互斥量与临界区
mutex类， lock_guard 与unique_lock， std::lock_guard 不能显式的调用 lock 和 unlock， 而 std::unique_lock 可以在声明后的任意位置调用， 可以缩小锁的作用范围，提供更高的并发度。  
```c++
#include <iostream>
#include <mutex>
#include <thread>

int v = 1;

void critical_section(int change_v) {
    static std::mutex mtx;
    std::lock_guard<std::mutex> lock(mtx);

    // 执行竞争操作
    v = change_v;

    // 离开此作用域后 mtx 会被释放
}

int main() {
    std::thread t1(critical_section, 2), t2(critical_section, 3);
    t1.join();
    t2.join();

    std::cout << v << std::endl;
    return 0;
}
```
期物 future
future提供了一个访问异步操作结果的途径，可以作为屏障
```c++
#include <iostream>
#include <future>
#include <thread>

int main() {
    // 将一个返回值为7的 lambda 表达式封装到 task 中
    // std::packaged_task 的模板参数为要封装函数的类型
    std::packaged_task<int()> task([](){return 7;});
    // 获得 task 的期物
    std::future<int> result = task.get_future(); // 在一个线程中执行 task
    std::thread(std::move(task)).detach();
    std::cout << "waiting...";
    result.wait(); // 在此设置屏障，阻塞到期物的完成
    // 输出执行结果
    std::cout << "done!" << std:: endl << "future result is " << result.get() << std::endl;
    return 0;
}
```

原子操作
std::atomic 
原子操作是在多线程程序中“最小的且不可并行化的”操作
当然，并非所有的类型都能提供原子操作，这是因为原子操作的可行性取决于 CPU 的架构以及所实例化的类型结构是否满足该架构对内存对齐 条件的要求，因而我们总是可以通过 std::atomic<T>::is_lock_free 来检查该原子类型是否需支持原子操作，例如：
  
```C++
#include <atomic>
#include <iostream>

struct A {
    float x;
    int y;
    long long z;
};

int main() {
    std::atomic<A> a;
    std::cout << std::boolalpha << a.is_lock_free() << std::endl;
    return 0;
}
```
### 3月18日
#### 进程和线程简介
  进程是资源分配的最小单元， 拥有程序控制块(PCB)包含进程信息的描述信息和控制信息，是进程存在的唯一标志。  
  线程是任务调度的最小单位，任务调度由操作系统完成， 采用时间片轮转的抢占式调度方式， 线程共享进程的地址。  
  一个标志的线程由线程ID、当前指令指针、寄存器和堆栈组成。 进程由内存空间（代码，数据，进程空间，打开的文件）和一个或多个线程组成。  
#### 进程与线程的区别
  1. 线程是程序执行的最新单元，进程是操作系统分配资源的最小单元
  2. 一个进程是由多个线程组成
  3. 进程之间相互独立，但统一进程下的各个线程之间共享程序的内存空间以及一些进程级的资源（打开文件，信号）
  4. 调度和切换， 线程上下文切换比进程要快， 为什么，因为进程要切换虚拟地址，而线程不需要。 为什么虚拟地址切换慢？   
  现在我们已经知道了进程都有自己的虚拟地址空间，把虚拟地址转换为物理地址需要查找页表，页表查找是一个很慢的过程，因此通常使用Cache来缓存常用的地址映射，这样可以加速页表查找，这个cache就是TLB，Translation Lookaside Buffer，我们不需要关心这个名字只需要知道TLB本质上就是一个cache，是用来加速页表查找的。由于每个进程都有自己的虚拟地址空间，那么显然每个进程都有自己的页表，那么当进程切换后页表也要进行切换，页表切换后TLB就失效了，cache失效导致命中率降低，那么虚拟地址转换为物理地址就会变慢，表现出来的就是程序运行会变慢，而线程切换则不会导致TLB失效，因为线程线程无需切换地址空间，因此我们通常说线程切换要比较进程切换块，原因就在这里。  
  #### 多线程与多核
  多核(心)处理器是指在一个处理器上集成多个运算核心从而提高计算能力，也就是有多个真正并行计算的处理核心，每一个处理核心对应一个内核线程。   
  电脑双核四线程是采用超线程技术，将一个物理处理核心模拟成两个逻辑处理核心， 所以在操作系统中看到的CPU数量是实际物理CPU数量的两倍
  #### 协程
  用户管理的的用户空间线程，具有对内核来说不可见的特性。一个线程可以拥有多个协程。 协程的目的就是当出现长时间的I/O操作时，通过让出目前的协程调度，执行下一个任务的方式，来消除ContextSwitch上的开销  
  协程特点：  
  1. 线程切换由操作系统负责，协程由用户自己调度，减少了上下文切换，提高了效率。
  2. 线程默认stack是1M, 而协程更轻量，接近1k，
  3. 由于在同一个线程上，因此可以避免竞争关系而使用锁
  4. 适用被阻塞的，且需要大量并发的场景。 不适用于大量计算的多线程。
  
  协程原理：
  当出现IO阻塞的时候，由协程的调度器进行调度，通过将数据流立刻yield掉（主动让出），并且记录当前栈上的数据，阻塞完后立刻再通过线程恢复栈，并把阻塞的结果放到这个线程上去跑，这样看上去好像跟写同步代码没有任何差别，这整个流程可以称为coroutine，而跑在由coroutine负责调度的线程称为Fiber。比如Golang里的 go关键字其实就是负责开启一个Fiber，让func逻辑跑在上面。  
  
### 3月19日
  weg go 项目 , go 环境安装：https://linuxize.com/post/how-to-install-go-on-centos-8/  
  go models概念，编译时出错 package mymath is not in GOROOT (/usr/lib/golang/src/mymath) 
  
