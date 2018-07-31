# redis-cluster
...
The set up is 1 master and 2 slaves. 
To bring the cluster up, run:
```
vagrant up
```
# to test the setup 
ssh into master 
```
vagrant ssh redis01
```
connect to master and run
```
>redis-cli -h 127.0.0.1 -p 6379
>AUTH redis_password
should see OK
>INFO
```
Should see like below:

```
# Replication
role:master
*connected_slaves:2*
slave0:ip=10.11.8.3,port=6379,state=online,offset=407,lag=1
master_repl_offset:407
repl_backlog_active:1
repl_backlog_size:1048576
repl_backlog_first_byte_offset:2
repl_backlog_histlen:406
.....
```
# test  slaves
ssh into slaves
```
>vagrant ssh redis02
>INFO
```
should see below:

```
# Replication
role:slave
master_host:10.11.8.2
master_port:6379
master_link_status:up
master_last_io_seconds_ago:3
master_sync_in_progress:0
slave_repl_offset:1401
slave_priority:100
slave_read_only:1
connected_slaves:0
master_repl_offset:0
repl_backlog_active:0
repl_backlog_size:1048576
repl_backlog_first_byte_offset:0
repl_backlog_histlen:0
.......
```
