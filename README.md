# Cloud Database
A distributed and replicated storage service using Java.

## Approach
* Storage servers (KVServers) are monitored and controlled by an External Configuration Service (ECS).
<img src="https://user-images.githubusercontent.com/41395198/75396662-5a185f80-58f5-11ea-80e9-9d4573221f79.png" height="314" width="490.5">

* Tasks are assigned to KVServers by consistent hashing.

* Virtual nodes are introduced to achieve load balancing. 

  In figure below, we can observe that the 3 servers were not evenly distributed initially, server A handling 2 tasks, server B handling 1 task and server C handling 3 tasks. However, with the introduction of virtual nodes (A*, B*, C*), each server handles 2 tasks, achieving a more balanced distribution of workload.
<img src="https://user-images.githubusercontent.com/41395198/75396611-37864680-58f5-11ea-9a9e-dc94240624f9.png" height="248.5" width="490.5">

* Eventual consistency is achieved by using an optimistic lazy Multi-Primary Replication using gossiping protocol.

* KVservers can implement any of the 3 caching strategies:
 1. FIFO (First In First Out)
 2. LFU (Least Frequently Used)
 3. LRU (Least Recently Used)
 
 * Authentication and authorisation is also implemented based on the OAuth authentication framework, using JSON Web Token (JWT) as the access token.
 <img src="https://user-images.githubusercontent.com/41395198/75397741-e6c41d00-58f7-11ea-9520-2ba8f7d68b9b.png" height="314.5" width="490.5">

## How to run
1. Build project by running 
```console
mvn package
```
2. Start ECS e.g. 
```console
java -jar target/ecs-server.jar  -l ecs.log –ll ALL -a 192.168.1.1 -p 5152
````
3. Start KVServers e.g. 
```console
java -jar target/kv-server.jar -l kvserver1.log -ll ALL -d data/kvstore1/ -a 192.168.1.2 -p 5153 -b 192.168.1.1:5152
```
4. You can add interact with the database using the client
```console
java -jar target/echo-client.jar
```
