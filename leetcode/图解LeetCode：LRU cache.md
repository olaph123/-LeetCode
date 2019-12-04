这是LeetCode中第146号题目，不多说，这也是一个准备面试的同学必备的一道题目，出镜率非常高。

那么什么是LRU cache呢，假设有一个大小为6的数组，最开始保存的是[1,2,3,4,5,6]，如果这个数组就是LRU cache的话会有这样的性质：

![1574325101095](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1WialvM3u0nrUgIaoP0NwbAlDHAKrnwUJwHAGIIWbUoQRak6EX2BT8rDOYX7YfiaXvjpmBQ82ibnrSQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

一图胜千言，现在你应该明白什么是LRU cache了吧。

LRU的全称是Least Recently Used，也就是最近最少使用，从上图中我们可以看出，cache中每当新增数据时，最末尾的数据会被替换掉，因为这个被替换掉的数据就是最近最少使用的，这就是LRU这个名字的由来，LRU思想广泛应用于各种cache中。

题目要求的是设计LRU cache，支持get(key)和put(key, value)操作，这里的get和put就指的是上图中的“访问”和“新增”，key就是数组的下标，value就是数组的值。要求这两个操作都要在O(1)时间内完成。

知道了题目的要求，该如何设计这种数据结构呢？



#### 思考过程

首先让我们来考虑get操作，当从LRU cache中get一个数的时候需要将这个数组放到所有数据的最前端，如果我们就使用数组来当做LRU cache的话，那么每次get一个key的时候我们都需要将该key之前的所有数据后移一位，如图所示：

![1574325855112](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1WialvM3u0nrUgIaoP0NwbAU2kia2sCV9o9xUoKVLicQhMpSTxZkwFt1njxoic84dsyGwnemkaEyBggA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这样get操作的时间复杂度就是O(n)了，这显然不满足题目要求，那么什么样的数据结构移动元素的时间复杂度是O(1)呢，很显然是**双向链表**，如果你想不到这一点请好好反思数据结构课的时候王者荣耀升到第几级了 :)  ，如图所示：

![1574335525606](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1WialvM3u0nrUgIaoP0NwbAQQYfpnjjw5oFOa9Jx2EvRnpfDuDiasDa5ZwR41YSh84ntcSibdXXFYZA/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

使用双向链表仅仅需要修改四个指针就可以了，我们无需修改3、4、5相关的指针，**这是双向链表最棒的一点**。

有了双向链表get操作就满足一部分要求了，不要忘了get(key)需要我们找到key对应的value，那么什么样的数据结构支持我们能在O(1)的时间内找到一个key对应的value呢，没错这就是map，如果你又没有想到这一点请好好反思一下数据结构课的时候吃鸡游戏升级到最新版了没有 :) 如图所示：

![1574336101903](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1WialvM3u0nrUgIaoP0NwbAibNcibxSE29J7SECPEnHn7vTcU6icUSHKeSJF5icPOY34lFvk6cN8o5j5w/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



有了双向链表和map，我们的get操作就可以在O(1)时间内完成移动和检索了，我们使用两种很常见的数据结构：双向链表和map，构建出了LRU cache，很有趣吧。

有了以上分析，代码就很清晰了。



#### 代码实现

```
class LRUCache {
public:
    LRUCache(int capacity): cap(capacity){}
    void use(int key, int value) {
        if (pos.find(key)!=pos.end()) {
            cache.erase(pos[key]);
        } else if(cache.size() >= cap) {
            int last_key = cache.back().first;
            cache.pop_back();
            pos.erase(last_key);
        }
        cache.push_front(pair<int,int>(key, value));
        pos[key] = cache.begin();
    }

    int get(int key) {
        if (pos.find(key)!=pos.end()){
            int value = (*pos[key]).second;
            use(key, value);
            return value;
        }
        return -1;
    }
    
    void put(int key, int value) {
        use(key,value);
    }
    list<pair<int,int>>cache;
    map<int, list<pair<int,int>>::iterator> pos;
    int cap;
};
```



#### 总结

实际上有时很多复杂的数据结构仅仅就是一些基础数据结构的组合，这里的关键在于你需要知道哪些数据结构可以拿来使用，如果你对这些数据结构理解的不是那么透彻的话，那么这类题目的难度就可想而知了。
