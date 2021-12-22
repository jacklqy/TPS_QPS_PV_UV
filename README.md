# TPS_QPS_PV_UV
QPS/TPS/PV 之间的关系。面试官：你做过的系统中，用户量是多少？你们项目(单个接口)最大的并发量是多少？用了多少台服务器进行负载？怎么判断这个系统需要多少台服务器？

## 概念解读
* QPS:（Queries Per Second），及每秒执行的查询总数（每秒有多少的请求响应--“每秒查询率”）。客户端请求一个地址时，比如百度首页，其实会产生很多的请求，比如js、css、png等，像这样的每个单个请求都可以算作查询次数。若在一秒内，客户端请求服务端的首页，服务端返回了N个内部链接（js、css、png、html等），那么服务端的QPS就为N。QPS反映系统的吞吐能力，更偏向于读取文件，查询数据。

* TPS:（Transactions Per Second），即每秒执行的***事务***总数--事务数/秒。它是软件测试结果的测量单位。首先一个事务包括三个动作，即客户端请求服务端，服务端内部进行处理，服务端对客户端进行响应。将这三个动作看成一个整体，并将之称为一个事务，若在一秒内，服务端可以完成N个事务，则这个服务端的TPS为N。一般来说，评价系统的性能主要看系统的TPS，系统的整体性能取决于性能最低模块的TPS值。（木桶的容量取决于最短板）

* 简易区分QPS/TPS：若在一秒内，用户请求了百度首页并看到了首页全貌，这样就形成了一个TPS，但却形成了多个QPS。(1T = 多Q)。若在一秒内，我们请求一个单调的网页，此网页只有一个html，不包含任何其他内部链接，此时TPS=QPS。

* RPS:即Requests Per Second的缩写，每秒能处理的请求数目。等效于QPS。

* PV:即 page view，页面浏览量。用户每一次对网站中的每个页面访问均被记录1次。用户对同一页面的多次刷新，访问量累计。

* UV:即 Unique visitor，独立访客。通过客户端的cookies实现。即同一页面，客户端多次点击只计算一次，访问量不累计。

* IP:即 Internet Protocol，本意本是指网络协议，在数据统计这块指通过ip的访问量。即同一页面，客户端使用同一个IP访问多次只计算一次，访问量不累计。

* UV、IP的区别：比如你是ADSL拨号上网，拨一次号自动分配一个IP，进入了网站，就算一个IP；断线了而没清理Cookies，又拨号一次自动分配一个IP，又进入了同一个网站，又统计到一个IP，这时统计数据里IP就显示统计了2次。UV没有变，是1次。同一个局域网内2个人，在2台电脑上访问同一个网站，他们的公网IP是相同的。IP就是1，但UV是2。

## 压力测试
* 压力测试分两种场景：一种是单场景，压一个接口的；
* 第二种是混合场景，多个有关联的接口。压测时间，一般场景都运行10-15分钟。如果是疲劳测试，可以压一天或一周，根据实际情况来定。

