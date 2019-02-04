[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](https://opensource.org/licenses/MIT)

# System Design and Scalability

- Scaling
  - **Vertical Scaling**: add resources to a single node system. Expensive and there is limitation
  - **Horizontal Scaling**: infinite hosts but have to deal with distributed systems. *More preferred*
- **Load Balancing**: sit in front of service and delegate the client request to one of the nodes behind the services. Could be based on round robin or load of the nodes behind the service. Load balancer has public ip address while the backend has private.
  - Layer
    - L4: considers both client and destination ip addresses and port numbers to do routing
    - L7: uses http uri to do routing. *More common*
  - Clustering
    - Active-active: two or more servers aggregate the network traffic load and working as a team distributes it to the network servers.
    - Active-passive: primary load balancer distributes network traffic to suitable server, while second load balancer operates in listening mode to constantly monitor the performance of the primary load balancer, ready at any time to step up and take over load balancing duties if the primary load balancer is having difficulty or failing.
- **Caching**: used to speed up requests. If data is accessed frequently store it there so it can be retrived quickly. Example: user sessions, fully rendered blog articles, activity streams, user<->friend relationships
  - Consider: cache data may be inconsistent, cache data has to be small, eviction policy of cache such as - Least Recently Used (LRU), Least Frequently Used (LFU), Most Recently Used (MRU)
  - **Distributed Cache**: hold data in memory. Not source of truth and can hold limited amount of data(based on size of memory of host)
    - **Memcached**: simple, fast key value storage
    - **Redis**: same as memcached but can do more. Can be set up as a cluster to increase availability and data replication
- **Readundant Array of Independent Disks(RAID)**: assumes have multiple hard drives.
  - **RAID 0**: segment logically sequential data between 2 hard drives (striping). This decreases write time
  - **RAID 1**: mirror data, write data in two places in parallel. This way even if one drive dies the other can still serve data.
  - **RAID 5**:  4-5 drives and one is used for redundancy. If one fails can be retrieved
  - **RAID 6**: 4-5 drives and one is used for redundancy. If 1-2 fails can be retrieved
  - **RAID 10**: four drives. Combination of Raid 0 and Raid 1, where it provides both striping and redundancy.
- **Database Replication**: frequent, automatic copying of data from a database of one computer or server to a database in another
  - **Master-slave**: master has one or more slave attached to it. Slaves keep copy of every row on master. So master and slave identical. This makes it so if one database dies there are identical backups. All databases can do read operations, but only masters can do write.
  - **Master-Master**: same as master-slave but there are more masters present. If one master fails, other masters continue to update the database.
- **Database Partition**: division of logical database into distinct independent parts. This is done for manageability, performance or availability reasons, or load balancing.
- **Performance vs Scalability**
  - Service is scalable if resources added to system results in increased performance in a manner proportional to the resources added.
  - Performance problem if system is slow for a single user.
  - Scalability problem if system is fast for a single user but slow under heavy load.
    - Scale up (vertical scaling), scale out (horizontal scaling)
- **Latency vs Throughput**
  - Latency: time to perform some action or to produce some result. Latency us measured in units of time - hours, minutes, seconds, nanoseconds, or clock periods.
  - Throughput: number of actions or results per unit of time. This is measured in units of whatever is being produced per unit of time.
  - Aim for maximal throughput with acceptable latency.
- **Availability vs Consistency**
  - **Consistency**: every read receives the most recent write or an error
    - Weak Consistency
    - Eventual Consistency
    - Strong Consistency
  - **Availability**: every request receives a response, without a guarantee that it contains the most recent version of the information
    - Fail-over
    - Replication
  - **Partition Tolerance**: system continues to operate despite arbitrary partitioning due to network failures. Only network failure will cause system to respond incorrectly
  - **Brewer's CAP theorem**. In distributed system only support two of three at any given point in time. In centralized system we don't have network partions, so we can get both availability and consistency.
    - **Consistency and partition tolerance (CP)**: data is consistent between all nodes, and maintains partition tolerance (preventing data desync) by becoming unavailable when a node goes down. Waiting for a response from a partitioned node might result in a timeout error. CP is good choice if business needs atomic reads and writes. Use: HBase, MongoDB, Redis, MemcacheDB, BigTable-like systems
    - **Availability and Partition Tolerance (AP)**: nodes remain online even if they can't communicate with each other and will resync data once the partition is resolved, but you aren't guaranteed that all nodes will have the same data (either during or after the partition). Responses return the most recent version of the data available on a node, which might not be the latest. Writes might take some tome to propagate when the partition is resolved. AP is a good choice if the business needs allow for eventual consistency or when the system needs to continue working despite external errors. Use: Voldemort, Riak, Cassandra, CouchDB, Dynamo-like systems
    - **Consistency and Availability (CA)**: not possible, it is a software tradeoff. You can choose what to do in the face of a network partition. Nodes remain online even if they can't communicate with each other and will resync data once the partition is resolved, but you aren't guaranteed that all nodes will have the same data (either during or after the partition). Use: Traditional relational databases like PostgreSQL, MySQL, etc.
- **ACID**: Set of properties of relational databse transactions intended to guarantee validity in the event of errors, power failures, etc.
  - **Atomicity**: each transaction is all of nothing
  - **Consistency**: any transaction will bring the database from one valid state to another
  - **Isolation**: executing transactions concurrently has the same results as if the transactions were executed serially.
  - **Durability**: guarantees that once a transaction has been committed, it will remain committed even in the case of a system failure (e.g., power outage or crash).
- **BASE**: Basically Available, Soft state, Eventual consistency. BASE system chooses availabilty over consistency. It describes property of NoSQL database
  - **Basically available**: gurantees availability
  - **Soft state**: state of the system may change over time, wven without input
  - **Eventual consistency**: the system will become consistent over time, given that the system doesn't receive input during that time.

## Sources

- [System Design Primer](https://github.com/donnemartin/system-design-primer)
- [Awesome Scalability](https://github.com/binhnguyennus/awesome-scalability)
- [System Design Interview](https://github.com/checkcheckzz/system-design-interview)
- [System Design](https://github.com/shashank88/system_design)
