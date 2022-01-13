#  Substrate offchain worker practic for Substrate Learnning Course Lesson 4

#### Name: 彭亚伦
#### Phase: 4th
#### Team: #2
#### SID: 022


## Screenshots


### Run 

![offchain worker running](https://raw.githubusercontent.com/Arstman/imgstorage/main/offchain-work-run.png)

## 提交数据上链的方法及原因
使用submit unsigned transaction with payload 方法来提交数据上链, 原因如下:
1. 采用signed transaction提交会产生费用, 长期运行带来巨额开销, 而价格信息本身是免费资料, 且链上数据我们已经确定只存最新10个价格, 存储成本不算高, 为此花费过多没有必要
2. 如果但存使用unsigned transaction提交, 那么相当于任何账户都可以对此提交, 则很容易遭受DDOS类似攻击, 且很难追溯,不可取 
3. 而unsigned transaction with payload 方法提交则比较平衡, 由于能提交的账户是预先runtime里面注入的, 因此具备一定的风险可控性, 且发现滥用也相对比较容易追溯

综上, 采用unsigned transaction with payload 方法提交是比较合理的选择.

## 可以继续优化的点
1. 目前是抓取价格后直接上链, 实测api接口一天只给200个调用次数, 且有频率限制; 因此实际可以通过local storage 来缓存一定时期内的价格, 在该时间内, 不去抓取数据而是直接从local storage里面提取, 可能要用到timestamp来规划
2. 目前抓取的价格数据到上链数据, 是通过 `Vec<u8> -> &str -> Vec<&str> -> (u64, Permill)` 的路径转换而来, 略显不够优雅, 但Permill没有太多资料, 源码里面又各种宏满天飞, 还有待研究后改进.
