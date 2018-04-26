System Design

Ask good questions 
- What features to work on (min viable product)
- How much should the system be scaled
- Dont use buzzwords
- Clear and organized thinking
- Drive discussion (80-20 rule)

- Features
- Define APIs
- Availability
- Latency performance
- Scalibility
- Durability
- Class Diagram
- Security and Privacy
- Cost effective


Concepts
- Vertical or horizontal scaling
	- vertical scaling can be expensive, but no distributed system issues.
	- hor scaling you can infinitely keep adding host, but problems related to Dist. system arise.

- CAP theoram
	- C : your read has the most recent write
	- A : you will get a response, but it may or may not be the most recent write.
	- P :  Between 2 nodes you could dropping n/w packets
	- Traditional DBs choose consistency over availability. NoSQL prefer A over C.

- ACID vs BASE
	- ACID : Atomicity, Consistency, Isolation, Durability
	- BASE : Basically Available Soft State Eventual Consistency
	- ACID is more in traditional DBs, BASE in NoSQL

- Partitioning/Sharing data.
	- Every node of DB is response of some part of trillions of records.
	- One way of doing it is consistent hashing.

- Optimistic vs Pessimistic locking

- Strong vs Eventual Consistency

- Relational DB vs NoSQL

- Types of NoSQL
	- Key value
	- Wide column
	- Document based
	- Graph based

- Caching

- Data center, Racks, Hosts
	- How racks communicate
	- What if racks or the entire DC goes down ?

- CPU, Memory, Hard drive, Network bandwidth
	- These are limited resource, you need to consider them when designing for scale.

- Random vs Sequential read/write on disk.
	- Sequential is faster than random

- http vs http/2 vs websockets

- TCP/IP model

- ipv vs ipv6

- TCP vs UDP
	- UDP : Streaming
	- Document upload : TCP

- DNS lookup

- HTTPS and TLS

- Publick Key infrastructure & Certificate authority

- Symmetric vs Asymmetric encryption

- Load Balancer -> L4 vs L7

- CDNs and Edge

- Bloom Filter and count-min sketch

- Paxos : Consensus over distributed hosts (leader election)

- Design patterns & object oriented design

- Virtual machine and containers

- Publisher-Subsciber over Queue

- Map reduce
	- distributed and parallel processing of big data

- Multi-threading, concurrency, locks, synchronization, CAS

Tools
- Cassandra 
	- Wide column, highly scalable DB.
	- Storing Simple Key value storage
	- Storing time series data
	- Traditional rows with many columns.
	- Can provide both eventual and strong consistency
	- Use consistent hashing to shard data
	- Uses gossiping to inform all nodes about the cluster

- MongoDB / CouchBase
	- JSON like document
	- ACID properties at document level
	- Scale well

- MySQL
	- Master Slave architecture - Scales well
	- Many tables and relationships
	- Full set of ACID properties

- Memcached / Redis 
	- Distributed cache, hold data in memory
	- Redis can be clustered for more availablility and data replication.
	- Redis can flush data on hard drive.
	- They shouldnt be source of truth

- Zookeeper
	- Centralized config management tool
	- Scales well for read, but not for write.
	- Stores data in memory

- Kafka
	- High fault tolerant queue used in pub-sub or streaming apps

- NGINX and HAProxy
	- They are efficient load balancers
	- NGINX can handle tens of thousands of connection from clients in a single instance

- Solr and elasticsearch
	- Search platform built over lucene
	- Highly available and scalable and fault tolerant

- Blobstore like Amazon S3
	- For file storage

- Docker
	- Kubernetes
	- Mesos

- Hadoop/Spark
	- HDFS

















