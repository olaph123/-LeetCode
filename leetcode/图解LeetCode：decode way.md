如果给你一个找规律的题目1,2,3,5,? 下一个是多少？

大家在读书时肯定遇到过这样找规律的题目，聪明的你肯定会知道下一个数字是8，为什么呢？这其实就是著名的斐波那契数列，下一个数字是前两个数字的和。

本质上LeetCode的第91号题目Decode Ways其实就是斐波那契数列数列的变型，这是个什么样的题目呢？

大写字母A-Z被映射到了以下数字：

```
'A' -> 1
'B' -> 2
...
'Z' -> 26
```

那么给定一个只包含数字的字符串，问这个数字字符串有多少种可能的解释。

比如给定“12”，那就有两种解释，可以解释为AB也就是(1, 2)，也可以解释为L，即（12）。

该怎么解决这个问题呢？



#### 思考过程

给定一个字符串假设是“1226”，我们可以先从最简单的情况开始，从右边数第一个数字是6，那么对于数字6来说很简单，只有一种解释，那就是6，即：

![1574828591101](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2elx5rDk7w0X4trick8HtYjQeEddUGhv9yhMc76eZg8vtFcCZhwWiaPbMYzkeRVbWichKIfiaDBpFlDw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

我们用“数组下标 ：{编码数字}”的形式来表示以某一个位置为开始的字符串所有可能的编码形式。

接下来我们来到了右数第二个数字2，对于26来说有两种解释，一种就是26也就是Z；另一种是(2,6)也就是BF，因此：

![1574829587052](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2elx5rDk7w0X4trick8HtYjpAI87Tz8ZJ7FtSyeQjZib3gHTL0n8hOoL96nkymib4yn2ZibJiaBa1xE9Q/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

这样我们来到了右数第三个数字，如果前两个我们可以很直观的“看出”答案的话，那么第三个数字所有可能的编码就不是那么直观了，显然我们需要**某种策略**，那么这个策略是什么呢？你能看出来吗？在往下读之前仔细想一想这个问题。

好啦，让我们来检查一下你想的是不是正确的(或者是不是已经困了)。

这里的策略就是要想知道第三个数字所有可能的编码形式我们**必须借助前两个数字所有的编码形式**，那这是什么意思呢？

对于右数第三个数字来说，我们可以将其解释为2，那么这时我们就必须依赖右数第二个数字形成的编码格式，从上图我们已经知道了该数字能形成的编码格式为{26},{2,6}，因此我们只需要将右数第三个与{26}，{2,6}合并就可以了，如图所示：



![1574829648540](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2elx5rDk7w0X4trick8HtYj22hzTaE5wl3bOicS6XyGUl3b8aTicfyUgvLUJcSCiajYc2UAfj37icq0Tw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



对于右数第三个数字来说除了可以解释为2，我们还可以解释为22，如果解释为22，那么我们就必须依赖于右数第一个数字锁形成的编码格式，也就是{6}，即：



![1574829766000](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2elx5rDk7w0X4trick8HtYjCjUhNf4R5B1JCDRbj1oHibm2GAHDoaawICC6QLtJQ5Xl45dXudUveTQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

也就是说对于右数第三个数字来说所有可能的解释是{2,2,6}、{2,26}、{22,6}，如图所示：

![1574830130530](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2elx5rDk7w0X4trick8HtYjsS2wlQDYZw0wx56a3wBIQfR4lCwk1AONgYpv4bAADGXgIlT8BTuPyg/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)



现在你该知道怎么分析了吧，接下来我们来到了右数第四个数字1，基于上述讨论自己推导一下已1为开头的数字有多少种可能的解释。

分析完毕后用下图来验证一下吧。

![1574830363069](https://mmbiz.qpic.cn/mmbiz_png/8g3rwJPmya2elx5rDk7w0X4trick8HtYjLNAOR73mgUrTzONrUl4fL8wcAUtuF2B7D9XfeOPIBtDicYFQqtFXgmQ/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

怎么样，分析的正确吗？

接下来让我们数一数每一个位置所有可能的解释个数：

1. 右数第一个数字是1个
2. 右数第二个数字是2个
3. 右数第三个数字是3个
4. 右数第四个数字是5个

这就是我们在本文开头提到的那个数列，也就是著名的斐波那契数列，现在你应该明白了吧。



#### 公式提炼

为什么该问题会形成斐波那契数列数列呢？

注意，接下来是重点。

我们记以右数第i个数字为开头的字符串所能形成的编码种类个数为F(i)，那么如果我们单独将第i个数字单独解释，那么能形成的编码种类个数就取决于F(i-1)。

而如果我们将第i和和第i-1这两个数字单独解释，那么能形成的编码种类个数就取决于F(i-2)，当然，这里有个前提，那就是第i和和第i-1这两个数字形成的整数必须在10~26之间，这样我们才能将其解释称为一个大写字母，否则F(i)的值就只取决于F(i-1)，即：

```
F(i) = F(i-1) + F(i-2) if 10 <= substr(i,i+1) <= 26
F(i) = F(i-1)          else
```

现在你该知道了吧，原来这里确实存在一个稍微有点变型的斐波那契数列。

有了这些分析，还怕写不出代码吗？



#### 代码实现

以下仅仅将上述公式翻译成代码。

```
int numDecodings(string s) {
    	int len = s.length();
    if (len == 0||s=="0")
        return 0;
    vector<int>F(len+1, 1);
    for (int i = len - 1; i >= 0; i--){
        if (s[i] == '0' )
            F[i] = 0;
        else if (i == len - 1)
            F[i] = 1;
        else if (atoi(s.substr(i, 2).c_str()) <= 26)
            F[i] = F[i + 1] + F[i + 2];
        else
            F[i] = F[i + 1];
    }
    return F[0];
}
```



#### 总结

有时你会发现算法其实还是挺有趣的(前提是你能想出解决方法:) )，在这个题目中经过我们的分析后发现，这其实就是初中学过的斐波那契数列而已。可能我们一下不能想到其本质，但是我们从最简单的情况下开始着手分析并找出规律，最后通过规律提炼出公式才发现这其实是斐波那契数列的变型，用代码实现一个斐波那契数列是非常简单的，真正的价值在于这背后的分析过程，希望对你能有所启发。