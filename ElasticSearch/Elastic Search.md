ELK stack
- Elastic search
	- 
- Kibana
	- UI for dashboard, widgets or visualizations for log monitoring
- Logstash
	- Server side processing pipeline for data extraction and transform

-------------------
NoSQL JSON based Distributed database.
RESTful API support
Used to store / manage logs, metrics, application trace data

Documents, Terms and Index
Document is metadata + JSON


-------------------
**Inverted Index**

Terminology:
Cluster
- Set/Collection of Nodes running elastic search instances.
Node
- server running an elastic search instance
Index
- group of documents(data + metadata)
Shard
- independent instance
- primary and replica shard
Index template
- blueprint to create a index
- contains information on configuration of Index
Data mapping
- flexible or dynamic
Index alias
- An index alias is a set/group of index.
- configured via regex pattern
- write index
Data stream
-  it is an abstraction on top of index designed for append only time-series documents.
--------------------
document --> row in sql
fields --> columns in sql
index --> table in sql

keys in document are starting with underscore (_).
- REST API Interaction via Kibana UI.
- REST API Interaction via cURL in command line.

---------------------
###### Sharding in ElasticSearch
- Sharding is a way to divide indices into smaller pieces.
- Each piece is called shard.
- Sharding is done at index level.
- Sharding improves performance by parallelization of queries.
- Split API vs. Shrink API

###### Replication - copying the shards
- Replication group - group of shards (primary shard + replica shard)
- replication exists for fault tolerance and parallelization.
- primary shard and replication shard can not reside on the same Node.

Multiple nodes of elastic search on the single server.

Node Roles
- Master Node
	- The master node is responsible for lightweight cluster-wide actions such as creating or deleting an index, tracking which nodes are part of the cluster, and deciding which shards to allocate to which nodes.
- Data Role
	- Data nodes hold the shards that contain the documents you have indexed.
- Ingest Role
	- Enable to run ingest pipeline
	- manipulate documents
	- before adding index to them, like removing some useless fields and adding some more information.
	- It is just like a simplified version of logstash directly within ES
	- In case of file beat -> off
- ML Role
	- Machine learning jobs node
- Coordination Role
	- route queries to other nodes
	- load balancer
	- doesn't do anything itself
- Voting only
	- takes participate in electing master node


Useful queries

```
GET /_cluster/health

GET /_cat/nodes?v

GET /_cat/shards

PUT student

GET /_cat/indices?v

DELETE student

PUT student
{
  "settings": {
    "number_of_replicas": 2,
    "number_of_shards": 2
  }
}

POST student/_doc
{
  "name": "Veet",
  "age": 23
}

POST student/_doc/1234
{
  "name": "Rahul",
  "age": 24
}

DELETE student/_doc/1234

GET /student/_doc/1234

GET /student/_search
{
  "query": {
    "match_all": {}
  }
}

POST /student/_update/don2spQBBs4ctZ-RYG1X
{
  "doc": {
    "age": 35,
    "lastname": "Sharma"
  }
}

POST /student/_update/don2spQBBs4ctZ-RYG1X
{
  "script": {
    "source": "ctx._source.age=params.new_age",
    "params": {
      "new_age": 5
    }
  }
}

POST /student/_update/don2spQBBs4ctZ-RYG1X
{
  "script": {
    "source": """
      if(ctx._source.age< 0){
        ctx.op='noop';
      }else{
        ctx._source.age++;
      }
    """
  }
}

POST /student/_update/don2spQBBs4ctZ-RYG1X
{
  "script": {
    "source": "ctx._source.age++"
  },
  "upsert": {
    "name": "Mohit",
    "age": 10
  }
}

PUT /student/_doc/don2spQBBs4ctZ-RYG1X
{
  "name": "Uddhav",
  "age": 25
}
```

**Routing**
- A process of resolving a shard for a document.
- On which shard does the ES store the document.
- How does ES search for a document.

```
shard_num = hash(_routing) % num_primary_shards
```
- once we start creating documents then no. of shards can not be changed.


How data is read
- let say GET /student/\_doc/Id 
	- first it will go to coordination node from there it will go to data node and routing will give the result.
	- routing will give replication group as result.
		- ARS - adaptive replica selection

How data is written
- PUT /index/\_doc/id
	- coordination node ->  data node -> routing -> replication group 
	- data comes to primary shard and then it will be validated, post that it will get stored in primary shard
	- now it will get saved in replica shard

Global & local checkpoint
- Global checkpoint is for entire replication group
- Local checkpoint is for each replica
- Useful in case of node failure and data retrieval
- primary_term
	- counter of change of primary shard for the document
- sequence_number
	- counter for each write operation

Optimistic Concurrency control in ES
- to handle this, client can use seq_number and primary_term for control.
