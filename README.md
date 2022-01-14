#  Substrate offchain worker practic for Substrate Learnning Course Lesson 4

#### Name: 彭亚伦
#### Phase: 4th
#### Team: #2
#### SID: 022


## Screenshots


### Run 

![offchain worker running](https://raw.githubusercontent.com/Arstman/imgstorage/main/offchain-work-run.png)

## 提交数据上链的方法及原因
使用`submit unsigned transaction with payload` 方法来提交数据上链, 原因如下:
1. 使用`signed transaction`提交会产生费用, 长期运行带来巨额开销, 而价格信息本身是免费资料, 且链上数据我们已经确定只存最新10个价格, 存储成本不算高, 为此花费过多没有必要
2. 使用`unsigned transaction`提交, 那么相当于任何账户都可以对此提交, 则很容易遭受DDOS类似攻击, 且很难追溯,不可取 
3. 使用`unsigned transaction with payload` 方法提交则比较平衡, 由于能提交的账户是预先runtime里面注入的, 因此具备一定的风险可控性, 且发现滥用也相对比较容易追溯

综上, 采用unsigned transaction with payload 方法提交是比较合理的选择.

## 可以继续优化的点
1. 目前是抓取价格后直接上链, 实测api接口一天只给200个调用次数, 且有频率限制; 可以设置抓取时间间隔来保证; 或者换rate.xs等限制较少的数据源
2. 目前抓取的价格数据到上链数据, 格式转换路径还可以更优雅一些, 但Permill没有太多资料, 源码里面又各种宏满天飞, 还有待研究后改进.
