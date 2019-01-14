系统设计就是不断问自己问题，并解决问题的过程。有很强的套路性，只要练几个就能把规律摸熟：

第一问：设计的系统是干啥用的 (use cases)？
* User posts a tweet, which will be pushed to followers
* User views a user's timeline
* User views homepage
* User search

第二问：设计的系统有哪些限制条件？
* Posting a tweet should be fast
* Fanning out a tweet should be fast, unless there are millions of followers
* Viewing the timeline should be fast
* Searching should be fast

第三问：设计系统时有哪些假设？
* 用户总数：100 million daily active user
* 日访问量：500 million write per day with average fan out 10 deliveries. 10 billion read per day.

第四问：基于假设的估计?
* tweet size: 10KB, 5TB per day
* 5700 write requests per second, 115K read requests per second, 57K deliveries on fanout per day.

第五问：如何做high level design?
* client -> web server -> write api / read api -> fanout service / timeline service -> cache -> sql db

第六问：如何设计关键部件？(基于user case)
* user posts a tweet -> write api -> stores user's tweet -> fanout service finds followers -> store tweet in follower's timeline -> notification service to notify followers (async)
* user views home timeline -> user service to get all user's followers -> fetch all tweets

第七问：如何scale?
* Load balancer
* Cache
* DB sharding
