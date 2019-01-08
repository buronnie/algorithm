系统设计就是不断问自己问题，并解决问题的过程。有很强的套路性，只要练几个就能把规律摸熟：

第一问：设计的系统是干啥用的 (use cases)？
* 用户输入一些文字，提交后获得一个sharable的url
* 用户访问生成的url，可以看到文字内容

第二问：设计的系统有哪些限制条件？
* 用户：可以匿名使用
* url: 多久到期，还是一直有效，如果到期需要删除对应的内容

第三问：设计系统时有哪些假设？
* 内容只是文字
* 用户总数：10million
* 日访问量：10million read, 1million write
* 每个paste的大小：1KB content
* url: 7B

第四问：基于假设的估计?
* 每日生成的paste大小：1M * 1KB = 1GB, 一年就是365GB
* 每日url数量1M, 一年就是365M
* 116 read requests per second
* 12 write requests per second

第五问：如何做high level design?
* client -> web server -> write api / read api -> sql db

第六问：如何设计关键部件？(基于user case)
* 用户提交文字 -> write api -> generate unique url -> save data into paste table -> return url
* 用户访问url -> query paste table -> return content
* 从表开始设计：
- paste table: id, content, url, created_by, created_at

第七问：如何scale?
* Load balancer
* Cache
* DB sharding
