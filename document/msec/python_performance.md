## 运行环境

```
CPU: Intel(R) Xeon(R) CPU X3320  @ 2.50GHz  4核
内存：8GB
网卡：千兆网卡
Client与Server之间PING值：0.854ms

```

## 测试程序

- echo服务<br/>server将请求字符串直接返回
- 延时echo服务<br/>server先sleep 100ms, 再将请求字符串返回
- rpc-echo服务<br/>server将请求转发至server2, server2将请求字符串直接返回
- 延时rpc-echo服务<br/>server将请求转发至server2， server2先sleep 100ms, 再将请求字符串返回

备注：请求字符串小于100字节

## TCP请求连接模式

- 短连接<br/>每个请求都需要建立TCP连接
- 长连接<br/>建立TCP连接后，保持不断开，连续发送请求

## 程序A测试结果(QPS及延时分布)

| | QPS | CPU | <5ms | 5 ~ 10ms | 10 ~ 30ms| 30 ~ 50ms | >50ms |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 长连接 | 32,000/s | 89% | 99.92% | 0.07% | 0.01% | 0 | 0 |
| 短连接 | 25,000/s | 96% | 98.68% | 1.19% | 0.14% | 0 | 0 |


## 程序B测试结果QPS

| QPS | CPU | <120ms | 120 ~ 150ms | 150 ~ 200ms | > 200ms |
| --- | --- | --- | --- | --- | --- |
| 2,900/s | 23% | 100% | 0 | 0 | 0 |

备注：该用例没有区分长连接和短连接，后端sleep 100ms，主要是依靠增加进程数来提高处理能力，由于框架本身存在的惊群效应等原因，这里配置为300个进程，如果继续增加，会导致时延加重。

## 程序C测试结果QPS

| | QPS | CPU | <5ms | 5 ~ 10ms | 10 ~ 30ms| 30 ~ 50ms | >50ms |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 长连接 | 12,000/s | 71% | 99.30% | 0.53% | 0.17% | 0 | 0 |
| 短连接 | 10,000/s | 72% | 98.06% | 1.94% | 0 | 0 | 0 |


## 程序D测试结果QPS

| QPS | CPU | <120ms | 120 ~ 150ms | 150 ~ 200ms | > 200ms |
| --- | --- | --- | --- | --- | --- |
| 2,900/s | 36% | 100% | 0 | 0 | 0 |

备注：该用例没有区分长连接和短连接，后端sleep 100ms，主要是依靠增加进程数来提高处理能力，由于框架本身存在的惊群效应等原因，这里配置为300个进程，如果继续增加，会导致时延加重。
