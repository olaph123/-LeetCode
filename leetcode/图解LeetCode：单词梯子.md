这是LeetCode的第127号题目Word Ladder，也就是单词梯子，那这个题目是什么意思呢？

给定两个单词，一个为开头单词beginWord，一个为结尾单词endWord，并且给定一个单词列表，找出从开头单词到结尾单词的最短转换路径，转换规则是这样的：

1. 一次只能转换单词中的一个字符
2. 每次转换后的单词必须存在于单词列表中

比如给定开头单词“hit”，结尾单词“cog”，单词列表["hot","dot","dog","lot","log","cog"]，那么最短的转换路径是这样的："hit" -> "hot" -> "dot" -> "dog" -> "cog"，因此应该返回5，如果不存在这样的一个转换路径那么返回0。

这个问题该如何解决呢，你有思路吗？



#### 思考过程

这个问题本质上是一个搜索问题，从开头单词到结尾单词可能存在多条转换路径，我们需要将最短的那个找出来，因此我们需要将所有的可能路径都计算出来，问题是该怎么计算呢？

很简单，如果你一下想不到该怎么找出一条可能的路径，**那么我们就从简单的第一步开始**，我们还是以本文开始示例来讲解，hit下一步转换后的单词一定出自单词列表(如果没有的话那么说明根本不存在这样一条转换路径，直接返回0就可以了)，如图所示：

![1574766791602](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1tL1ew6tDxbXCzWybuApGtf2HwzayuMDG8BGExlZ0CgKRsKR4nqqhRnDNDWLnnaus2hLOz0Jag8Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



但是，根据题目要求hit的下一步不能是dot、dog、lot、log以及cog，因为题目要求一次只能转换一个字符，因此从hit开始的下一步只能是hot，如图所以：

![1574759446030](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1tL1ew6tDxbXCzWybuApGt1RwoXP3YL3TiaYiaazn4LvKj97j0fX0sva8PXjvyfa4W2Az5OvznOgyg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



这样我们就来到了hot，同样的道理我们知道hot的下一步有两种可能，分别是dot和lot，如图所示：

![1574759501927](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1tL1ew6tDxbXCzWybuApGtVCvdbfPxUdykdMBejFGCx2KOj5wcfXClNog2Y2buR6UvzCtwqLG37Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



这样我们就来到了第三步，现在你应该明白了吧，从dot和lot开始继续上述过程，最终你能找到一条到达结尾单词的路径。

有的同学可能已经看出问题了，虽然这个方法可行，但是每次都要查一遍单词列表排除掉不符合题目要求的单词，这个过程非常耗时，该如何改进呢？



#### 建立索引

聪明的你应该想到了吧，为什么我们不提前计算出各个单词所有满足要求的转换单词呢，这样我们就不用在搜索过程中再去遍历单词列表了，实际上提前进行预处理也就是建立一个简单的索引，索引从key是各个单词，value是该key对应的所有可能的转换单词：


```
hot : dot,lot
dot : hot,dog,lot
dog : dot,log,cog
lot : hot,dot,log
log : dog,lot,cog
cog : dog,log
```

这样上述从hit到hot以及到{dot、lot}的搜索过程就如图所示了：

![1574759554739](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1tL1ew6tDxbXCzWybuApGtES0x2SDZz3e1zs1TR6Kj0pLu3dTmSxDfbb4ibne9mYLT8wC5vJh4SVQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



接下来我们来到了dot节点，通过提前建立的索引我们知道，dot的所有可能的下一转换单词是：hot,dog,lot，但是不要忘了其中的hot和lot是已经出现在之前的搜索路径中了，因此我们需要排除掉hot和lot，如图所示：

![1574759594357](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1tL1ew6tDxbXCzWybuApGtWzic1MPaG9JoUSNQAYJ0UYV1r2rl9iavU9vI5n9JgNaHIRWcRnN4EU6Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



接着让我们处理lot，从索引中知道lot的下一个转换单词是hot、dot、log，基于同样的道理我们应该排除掉hot和dot，因为这两个单词已经出现在搜索路径中了，这样我们得到了下图：

![1574759631841](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1tL1ew6tDxbXCzWybuApGtGX3IjDhhSHDeXsRPDfckr5mmRC5I9HGRGKT9dV04LCXmDRkFGYzmUw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

接下来我们来到了dog，从索引中知道dog的下一个转换单词是dot、log、cog，其中dot和log应该排除掉，同样因为之前已经出现过了，然后就是cog，Bingo，cog就是我们要找的结尾单词，如图所示：

![1574759660313](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1tL1ew6tDxbXCzWybuApGtwRYc5nzgyouoNvnEkZ1ksKialMVVWEMecia2Q0DEib98XgibgH70bkdjTg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



就这样我们找到了一条可行的路径，而且这条路径就是最短的，这是很显然的，因为我们是按照层的顺序来搜索的，因此只要能找到一条路径，那么就一定是最短的，这个最短路径就是"hit" -> "hot" -> "dot" -> "dog" -> "cog"，如图所示：

![1574759701629](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya1tL1ew6tDxbXCzWybuApGtSkey11c8oTGdWn39Tqxr4MXrvp1D9XXqjCNdwxlY25R7hww8ZuwR3A/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



这样问题就解决啦，有了上述分析，就可以写代码实现啦。



#### 代码实现

```
bool isM(string& a,string& b){
    int len = a.size();
    int num = 0;
    for(int i=0;i<len;i++)
        if (a[i]!=b[i])
            if ((++num) > 1)
                return false;
    return true;
}

int searchladder(string b, string e, map<string, bool>&p,
                 map<string, vector<string>>& mv) {
    vector<string>q;
    vector<string>tq;
    q.push_back(b);
    p[b]=true;
    int res = 0;
    while(!q.empty()){
        ++res;
        for(int i=0;i<q.size();i++){
          vector<string>& adj=mv[q[i]];
          for(int j=0;j<adj.size();j++){
            if (adj[j] == e){
                return res+1;
            }
            if (!p[adj[j]]) {
              p[adj[j]]=true;
              tq.push_back(adj[j]);
            }
          }
        }
        swap(q,tq);
        tq.clear();
    }
    return 0;
}

int ladderLength(string beginWord, string endWord, vector<string>& wordList) {
    bool is_e = false;
    int len = wordList.size();
    for(int i=0;i<len;i++){
        if (endWord == wordList[i]){
            is_e = true;
            break;
        }
    }
    if (!is_e)
        return 0;
    map<string, bool>p;
    map<string, vector<string>> mv;
    for(int i=0;i<len;i++) {
        for(int j=0;j<len;j++)
            if (i!=j && isM(wordList[i], wordList[j])){
                mv[wordList[i]].push_back(wordList[j]);
            }
        if (isM(beginWord, wordList[i])){
            mv[beginWord].push_back(wordList[i]);
        }
    }
    return searchladder(beginWord, endWord, p,mv);
}
```



#### 总结

这个题目思路上还是很直接的，关键一点在于预处理，也就是提前将每个单词所有可能的下一步转换计算出来，这将大大减少问题的搜索规模，因此这个题目的难点在于优化，实际上上述建立索引的过程还不是最优的，你能对其进一步优化吗？

