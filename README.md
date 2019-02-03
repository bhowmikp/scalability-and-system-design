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
- **Caching**: used to speed up requests. If data is accessed frequently store it there so it can be retrived quickly.
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

## Sources

- [System Design Primer](https://github.com/donnemartin/system-design-primer)
- [Awesome Scalability](https://github.com/binhnguyennus/awesome-scalability)
- [System Design Interview](https://github.com/checkcheckzz/system-design-interview)
- [System Design](https://github.com/shashank88/system_design)
