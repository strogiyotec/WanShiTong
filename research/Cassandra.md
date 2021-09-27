# Almas Abdrazak . Assignment 1
## Question one Data mining techniques in Cassandra vs Postgres
### 1. Abstract
The purpose of this research paper is to compare how Apache Cassandra(NoSql) is different from an open source DBMS Postgres.
It will cover storage patterns, load balancing and scalability techniques used by both technologies.

### 2. Introduction
Database Management Systems(DBMS) are widely used in modern technologies. Many companies rely on them in order to provide a high availability and performance web services for customers. 
According to Stackoverflow developers survey, Postgres is the most Loved SQL database among professional developers(1,Stackoverflow,2020). However, Postgres as most other traditional SQL databases provides a high data consistency which can lead to performance problems in specific loads. In order to eliminate this problem other types of SQL databases have evolved called NoSQL databases. Apache Cassandra is one of them. According to Cassandra's official site "Apache Cassandra is an open source NoSQL distributed database trusted by thousands of companies for scalability and high availability"(2, Apache Cassandra,2021). As you can see, the word "consistency" isn't here. 

### 3. Scalability Patterns
In this section, we first describe how scalability is achieved by Postgres which can be applied to other SQL databases. Then we will see how Cassandra is different and which patterns it uses to achieve high scalability.Finally we compare advantages and disadvantages of each solution. 

#### 3.1 Postgres
Postgres is a database written in C language which advertises itself as "The World's Most Advanced Open Source Relational Database".Postgres is used by Instagram as a main database that stores users data(3, Mike Krieger, Instagram 2013).
When we talk about scaling a Postgres there are many ways to achieve it(4, Postgres High availability chapter), but the most used one is Streaming Replication.
First, the main problem streaming replication is trying to solve is to distribute the load from a single database server.
According to AWS, a single EC2 server with 128 Gigabytes of RAM is more expensive than two servers with 64 Gigabytes each, so it brings a business value to distribute a load between multiple servers rather than buying a powerful one(5, AWS pricing page).
In order to achieve it, Streaming replication introduces two concepts, master and read replicas.
The workload uses the following algorithm, all updates to the database are going into a single database which is called master. Postgres uses a technique called Write Ahead Logging(WAL) which is an algorithm to eliminate data losses.
When a user inserts some data, Postgres first appends the changes into a single file(WAL), because appending data to disk in SSD is fast enough, users do not see any performance issues related to IO write. After that Postgres saves the changes in RAM which is a non-permanent storage. Saving changes in RAM makes INSERT operations fast enough, once RAM is full Postgres flushes the changes into the disk where each sql table is stored. However, if the server has been disabled because of any disasters, then once the server is alive it can restore the changes from the WAL file.

In terms of streaming replication, there could be multiple read only databases. Once data is saved in the master's WAL file, WAL's changes are replicated into all read replicas through TCP connections(one for each read replica). After that, all read requests from users can be directed to read only replicas which decreases the master's load. It has to be mentioned that by default replication strategy is asynchronous(6, Postgres Streaming replication) which means that once transaction in master's database was committed, it doesn't necessarily mean that all read replicas got changed. For use cases where a customer has to see an up to date changes, the read query can be fired into the master. The main point here is that all data will be stored in all servers eventually and there is no way to split the data from master server into multiple servers.

#### 3.2 Cassandra
As opposed to Postgres, **Cassandra** is not a highly consistent database.
Even though it's called NoSql , it doesn't have anything to do with a query language.
As was described above, Postgres has to keep all the data in each server, however, according to Cassandra Documentation "One important Cassandra attribute is that its databases are distributed"(2, Apache Cassandra documentation). Cassandra can be easily scaled by adding more servers, moreover those servers can have a different hardware.
Cassandra usually operates having many nodes, moreover, it supports masterless architecture as opposed to Postgres which means every node is a master(2, Apache Cassandra documentation). In order to split a dataset between nodes Cassandra uses an algorithm called Data Partitioning(2, Apache Cassandra documentation).When users want to insert some data, they have to specify a partition key for it which is an alternative to Sql based Primary key. This key then hashed in order get a numeric value. Each node in Cassandra is responsible for a range of these 
numeric values. The pseudocode in Python would look like this
```
def save value(partition_key,data):
    hash = hash(partition_key)
    node = get_node_by_hash(hash)
    node.save(data)
```
An official documentation doesn't mention it explicitly but Cassandra uses a consistent hashing.
Let's say that instead of numeric ranges we would use number of nodes as a module operator to choose a node where data will be saved.
The Python pseudocode is given below

```
def number_of_nodes = 5
def save value(partition_key,data):
    hash = hash(partition_key)
    node = nodes.get_node_by_id(hash%number_of_nodes)
    node.save(data)
```
In the code above we calculate a numeric value from a partition key and then use a modul operator to get a specific node , for example if the number was 7 then `7%5=2` which means we would use second node to store the data.
However this solution doesn't scale because if we add more nodes all hashes will be broken.
Let's say we added one more node so the total number is of nodes became 6. Now `6%6=1`, which means the hash for the same partition key gave us first node but the data was stored on the second node.
In order to avoid this problem Cassandra is using numeric ranges instead of modulo operator.
As was mentioned above, Cassandra is masterless, so every node can get an incoming traffic, generate a hash from partition key, save the data and distribute it over other nodes.


According to Cassandra documentation "Cassandra supports the notion of a replication factor (RF), which describes how many copies of your data should exist in the database"(2, Apache Cassandra Documentation).
Here is where the main difference between traditional Sql databases happens.
Data in Cassandra is splitted and partially replicated between servers.
This gives developers an ability to split huge datasets between servers and replicate the data in case when one of the servers was down(In other words high availability).
So how does Cassandra deal with consistency?
First of all Cassandra gives an SQL like query language to fetch or insert the data called Cassandra Query Language(CQL).
This is a great benefit because most developers in the market are already familiar with SQL syntax which decreases the learning curve for CQL.
When users fire a CQL query they have to specify a consistency factor.
According to official documentation "consistency level represents the minimum number of Cassandra nodes that must acknowledge a read or write operation to the coordinator before the operation is considered successful"(2, Apache Cassandra Documentation).
This gives a huge flexibility for developers.
If users have to see up to date values then consistency level can be increased which will decrease performance of a query.
Meanwhile If users are allowed to see  outdated values, the consistency level can be decreased which will give huge performance benefits. At the same time, in Postgres there is only one source of truth which is a master server that replicates itself to readers.

#### 3.3 Comparison
As we can see from the case study above,Postgres supports a replication feature 
in order to decrease the load from single server,
however the data itself is still strongly consistent while Cassandra chose a different approach,
each node can have a different values at the same time and application developers must choose a consistency level fetching the data from Cassandra. Which method is better primarily depends on business requirements and has to be considered by application developers.

### 4. Storage Patterns
In this section we are going to describe how data is stored in Postgres as opposed to Cassandra. Which formats are used by both databases and why.
#### 4.1. Apache Cassandra
In the world of databases there are primarily two main storage patterns which are designed for write and read heavy applications(7, Varun Jain).
First one is called B-Tree and it's mainly used by Sql database indexes to speed up read queries, the second one is called LSM-Tree which is used by most NonSql databases to speed up write queries.
Cassandra as NoSql database uses LSM-trees as a main data structure.
First of all, Cassandra doesn't support updates in a traditional way developers think about updates.
In essence Cassandra uses three components to store the data , namely : Commit log,Memtable,SSTables(8, Datastax Documentation, How Data is written). First of all Cassandra saves the data in commit log which behaves the same way as WAL in postgres(append only file). After that data is stored in RAM(this memory is called Memtable in Cassandra). Finally once Memtable is full , the data is flushed in file called Sorted String Table(SSTables). SStable is a file per table, each file contains table rows sorted by Partition Key which allows Cassandra to use binary search during selects(8, Datastax Documentation, How Data is written). 
During updates, Cassandra creates the second row with updated column values so it can happen that two SSTables contain same row with different values at the same time, however each row contains a specific timestamp column which specifies when this row was created, 
when users select the row , Cassandra fetches all row versions and chose the one with highest timestamp(9, Datastax Documentation, How is data updated).
Cassandra has a cron job which freezes the node and merges SSTables together into a single file , discarding all rows with outdated format, this process is called Compaction(10, Datastax Documentation, How is data maintained).
One thing to keep in mind, all SStables are sorted which means that merging process is sequential(read first entries from both files, compare them and write the lowest).
The sequential nature of this algorithm makes it fast for both SSD and HDD drives.
For deletion Cassandra just creates a new row with current timestamp, however this row will be marked as tombstone and during select as this row has the latest timestamp and tombstone marker, Cassandra will assume that row is deleted and won't return it to the user(11, Datastax Documentation, How is data deleted).

#### 4.2. Postgres
As opposed to Cassandra, Postgres is using B-tree as a main data structure to speed up search queries. All Postgres tables are stored in separate files, each row in a file has a 8 kb limit(12, Postgres Documentation, Database File Layout).
Same as Cassandra, Postgres can have multiple versions of the same row in a table file, however it doesn't use timestamps. Postgres keeps a transaction id of a transaction that created a row and during select queries, rows with transaction ids bigger than transaction id of a query will be omitted(13, Postgres Documentation,Routine Vacuuming).
In order to delete outdated rows, Postgres uses a process called Vacuum which marks the 8 kb row as outdated , and then new row can overwrite marked space in a file(13, Postgres Documentation, Routine Vacuuming).
As was said earlier Postgres extensively uses B-Tree structure. In order to speed up a search on rows file , an additional file called Index can be created that uses B-tree by default(14, Postgres Documentation, Indexes).
Index contains a column values specified during index creation as B-tree keys and row file offset as B-tree values. Consequently , every update first has to change the index and then create a new row version. This makes Postgres a good choice for read heavy operations but adds complexity for write heavy loads as opposed to Cassandra which is good for write operations but all reads have to make a 
binary search on all sstables which makes it slower on read heavy loads.

## 5. Conclusion
In Conclusion , both Postgres and Cassandra are great databases which serve 
different purposes. For high consistent ,read heavy traffic Postgres is a great choice with an ability to replicate a master server. At the same time, for non-high consistent requirements with high write traffic and requirements for High Availability, one should consider the masterless architecture of Cassandra with great write performance and great support for adding more nodes.

## References
1. Stackoverflow developers survey - https://insights.stackoverflow.com/survey/2020#technology-most-loved-dreaded-and-wanted-databases
2. Apache Cassandra official website - https://cassandra.apache.org/_/index.html
3. Instagram Engeneering Blog, Mike Krieger - https://instagram-engineering.com/handling-growth-with-postgres-5-tips-from-instagram-d5d7e7ffdfcb
4. Postgres, High Availability chapter - https://www.postgresql.org/docs/9.1/high-availability.html
5. AWS Pricing - https://aws.amazon.com/ec2/pricing/on-demand/
6. Postgres Streaming replication chapter - https://www.postgresql.org/docs/current/warm-standby.html
7. LSM-Trees and B-Trees:The best of two worlds, Varun Jain 2019 - https://dl.acm.org/doi/10.1145/3299869.3300097
8. Datastax Documentation. How data is written - https://docs.datastax.com/en/cassandra-oss/3.x/cassandra/dml/dmlHowDataWritten.html
9. Datastax Documentation. How is data updated - https://docs.datastax.com/en/cassandra-oss/3.x/cassandra/dml/dmlWriteUpdate.html
10. Datastax Documentation. How is data maintained - https://docs.datastax.com/en/cassandra-oss/3.x/cassandra/dml/dmlHowDataMaintain.html
11. Datastax Documentation. How is data deleted - https://docs.datastax.com/en/cassandra-oss/3.x/cassandra/dml/dmlAboutDeletes.html
12. Postgres Documentation. Database File Layout - https://www.postgresql.org/docs/9.4/storage-file-layout.html
13. Postgres Documentation. Routine Vacuuming -  https://www.postgresql.org/docs/9.4/routine-vacuuming.html
14. Postgres Documentation. Indexes - https://www.postgresql.org/docs/9.5/indexes-intro.html

## Question two. Usage example
Here I will show how to install and use Cassandra.

## Step one install via docker and run
```
docker pull cassandra:latest
docker run --name cassandra cassandra
```
![Docker](cas_docker.png)

## Step two create table
```
CREATE KEYSPACE IF NOT EXISTS store WITH REPLICATION = { 'class' : 'SimpleStrategy', 'replication_factor' : '1' };

-- Create a table
CREATE TABLE IF NOT EXISTS store.shopping_cart (
userid text PRIMARY KEY,
item_count int,
last_update_timestamp timestamp
);

-- Insert some data
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('9876', 2, toTimeStamp(now()));
INSERT INTO store.shopping_cart
(userid, item_count, last_update_timestamp)
VALUES ('1234', 5, toTimeStamp(now()));
```
![Table](create.png)

## Step three select query
```
 SELECT * FROM store.shopping_cart;
```
![Select](select.png)
