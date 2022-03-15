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

## 时间复杂度
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

### 进程与线程

