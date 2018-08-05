---
layout: post
title: Scaling with MySQL
subtitle: Do you really think that there is just one .db file in cluster of computers ?
tags: [scaling, software, distributed-system]
---
### Cluster of computers
![Beowulf-cluster](https://qph.fs.quoracdn.net/main-qimg-f78e88e58d74f6a67092381fc56b6baa-c){:class="img-responsive"}
### Before We start
It's totally theoritical I have no practical experience in this topic. I want that(practical experience) that's why I am writing this blog &#x1f609; .
### How to scale with MySQL ?
* Replication 
* Sharding

### Replication
In this process you replicate the main .db file to different machine. 
#### How general processes like update, insert, delete etc. works ?
Generally in every cluster there are two groups of server, master and slave.  
When using a mysql replication your application can read data from slave **BUT** but all modify operation occur through master server. All the update command like update, insert, delete etc. 
are sent to master, master records all request with date-time stamp(**executes them on itself as well**) in a log file called binlog. Now at any time(*I think it might be scheduled to check after x-seconds*) slave server can connect to master server and ask for binlog file by providing it's own last statement executed by that server. Server responds with requests after that statement sent by slave server. Hence it's a stateful transaction. 

#### What to do if Master fails ?
Logically take any slave(or the most updated one) and do update using binlog manually or automate that process and make it the new master with binlog of old master or You can also have multiple master and distribute the work that for insert statement go to master 1 or for update go to master-2. In the case of multiple master each master is technically a slave with greater permission, Just like humans. There will be other use case which we have to take care like we have to update master before updating slave, like you have to hold slave request before updating master B. But this logic will lead to bottle-neck.

#### Cons
* Not good for application like google docs, github, or pastebin, which is write-heavy application.
* If there is large data you will have storage issues.
* Slave can return stale(unupdated) data.

#### Other points to take care before we move to sharding
* Active Data set-> Set of all data that is frequently accessed by user. It is important to think about the active data set because too much active data will lead to increase in cache size. And what and how you define active is also important. Before setting threshold you have to check user pattern.

### Sharding
Divide the dataset in smaller part and assign each part to a single server. 
#### Sharding key
Key used to identify which data resides in which server. Such as in a counter-strike server, it might be like position of user1 is saved in one shard and sharding-key is user1 id. But you have to have some algorithm to map similar to hash table to map server and shard.
#### Cons
* It will be slow for queries involving multiple shard. 
* You lose ACID as whole not just in shards.
* You can not ensure that all shards are updated simultaneously
#### ACID Transactions
* Atomicity-> An atomic transaction is executed in it's entirety. Either it's executed or reverted.
* Consistency-> Every transaction transforms dataset from one consistent to other consistent state.
* Isolation -> It guarentees that transaction can run in parallel without affecting each other.
* Durability -> Once a transaction happens it is not lost.

### PostScript
Refer me a book or website or topic for this topic if you have some real life experience with these type of system.






