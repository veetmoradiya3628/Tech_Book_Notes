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

- update_by_query

```
POST /customers/_update_by_query
{
  "query": {
    "match_all": {}
  },
  "script": {
    "source": "ctx._source.age++"
  }
}
```
- delete_by_query

```
POST /customers/_delete_by_query
{
  "query": {
    "match": {
      "age": 26 
    }
  }
}
```

- Bulk API
	- Bulk API is a feature that allows you to index or update multiple documents in a single API call.
	- It is designed to be more efficient when dealing with large volumes of data, as it reduces the overhead of making individual requests for each document.
	- you provide a sequence of action and data pairs in a single HTTP request, and elastic search processes them in a batch.
	- Benefits of Bulk API

```
# index, create, update, delete
POST /customers/_bulk
{"index": { }}
{"name": "Shyam", "age": 30}
{"create": { }}
{"name": "Ajay", "age": 35}
{"update": { "_id": "hY71t5QBnM3DlPRp9Qha" }}
{"doc": {"age": 24}}
{"delete": {"_id": "ho4EuJQBnM3DlPRpfwg5"}}
```

- Analysis & Analyzer in Elastic Search
	- Analysis refers to the process of converting text into terms that can be efficiently stored and searched.
	- Elastic search uses a powerful text analysis engine to break down textual data into individual tokens or terms, which are then used to build an inverted index for fast and accurate full-text search.
	- Analyzer phases
		- Character filters
			- Character filters are used to process the input text before tokenization, They can perform tasks like removing HTML tags, replacing specific characters.
		- Tokenization
			- Standard tokenization splits text into words based on whitespaces and punctuation.
		- Token filtering
			- Lowercase token filter converts all tokens to lowercase.
	- we can make our custom analyzer

```
POST _analyze
{
  "text": "Hello, How are you ? What's up ? This is so high-end",
  "analyzer": "standard" # whitespace, stop
}

POST _analyze
{
  "text": "Hello, How are you ? What's up ? This is so high-end",
  "char_filter": [],
  "tokenizer": "standard",
  "filter": ["uppercase", "stop"]
}
```

- **Inverted index**
	- An inverted index is a fundamental data structure used to efficiently store and retrieve information from a large collection of documents. it is a backbone of Elasticsearch's powerful and fast full-text search capabilities.
	- The inverted index is designed to solve the problem of quickly finding all documents that contain a specific term or word in a vast collection of documents.
	- For each token, the inverted index maintains a list of document IDs where the token appears. It creates a mapping between each token and the corresponding documents that contain that token.
	- What is forward index ? its not commonly used.

- Numeric fields are stored in a different way compared to text field in Elasticsearch.
	- Instead of using the inverted index, Elasticsearch uses a data structure called "Doc Values" for numeric fields. This approach allows for efficient numeric range queries and aggregations.
	- Doc values are columnar data structures that store the values of each field for each document in a highly compressed and optimized format. Doc values are used for numeric fields, as well as some other field types like date fields.
	- When we index a document with two text fields, two inverted  indexes are created one for each text field.

- Mapping API
	- Defines how documents and their fields are stored and indexed.
	- Describes the data structure of your documents
	- data types of fields
	- how they should be analyzed.
	- dynamic mapping can be helpful for quickly getting started with ES but explicit mapping is essential for data control, consistency and optimizing the search performance.
		- it prevents type conflicts
		- validation rules and data integrity
	- dynamic parameter in mapping definition can have below values and it will behave accordingly
		- false
			- it will allow user to add document with additional field but those fields will not get consider for search so user can not search or query data based on the values of those keys.
		- strict
			- it won't allow user to add document with extra fields it mapping is defined at the strict level.

```
# get the mapping of all fields 
GET students/_mapping

# get the mapping of perticular field
GET students/_mapping/field/name

# explicit define the mapping for document in perticular index
PUT students 
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "age": {
        "type": "integer"
      }
    }
  }
}

```

 - If you want to update a mapping of a field then you have to create a new index and copy all the documents.

- Reindex API
	- Allows user to move documents from one index to other index with dynamic configuration (filter, mutation on document parameter if want to).
	
```
POST /students/_doc/2
{
  "name": "Shyam",
  "age": 6
}

GET /students/_search
{
  "query": {
    "match_all": {}
  }
}

GET /students/_mapping

PUT students_new
{
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "age":{
        "type": "integer"
      }
    }
  }
}

GET /students_new/_mapping


POST _reindex
{
  "source": {
    "index": "students"
  },
  "dest": {
    "index": "students_new"
  },
  "script": {
    "source": "ctx._source.id = ctx._source.name + ctx._source.age"
  }
}

GET /students_new/_search
{
  "query": {
    "match_all": {}
  }
}
```

- Coercion
	- The process of automatically converting data from one type to another during indexing or searching when the data type in the mapping does not match the data provided. Elasticsearch performs coercion to accommodate data that might not precisely match the expected data types defined in the mapping.
```
GET /_cat/indices?v

DELETE students

PUT students/_doc/3
{
  "name": "Uddhav",
  "age": 45
}

GET students/_search
{
  "query": {
    "match_all": {}
  }
}

PUT students
{
  "settings": {
    "index.mapping.coerce": false
  }, 
  "mappings": {
    "properties": {
      "name": {
        "type": "text"
      },
      "age": {
        "type": "integer",
        "coerce": false
      }
    }
  }
}
```

- Data Types in ES
	- Text - A data type for full-text search and analysis of text data.
	- Keyword - A data type for exact matching and filtering of keyword-like data, such as IDs, tags, or categories.
	- Date - A data type for representing dates stored as a long value, representing milliseconds since epoch.
	- Integer - A data type for storing 32-bit signed integers.
	- Long - A data type for storing 64-bit signed integers.
	- Float - A data type for storing single-precision 32-bit floating-point numbers.
	- Double - A data type for storing single-precision 64-bit floating-point numbers.
	- Boolean - true / false
	- Object - complex data type
	- Nested - Array of complex objects

- Index template
	- An index template in ES is a way to define a set of configurations and mappings that will be automatically applied to new indices that match a specific pattern.
	- index template are useful for automating the process of setting up and configuring indices in ES.
	- When a new index is created that matches the pattern defined in an index template, ES applies the template's settings, mappings, and other configurations to the new index, ensuring consistency across your data.
	
```
PUT _index_template/students_template
{
  "index_patterns": ["students_class_*"],
  "template": {
    "settings": {
      "number_of_shards": 2,
      "number_of_replicas": 1
    },
    "mappings": {
      "properties": {
        "name": {
          "type": "text"
        },
        "id": {
          "type": "keyword"
        },
        "dob": {
          "type": "date"
        }
      }
    }
  }
}

PUT students_class_1/_doc/1
{
  "name": "Ram",
  "id": "A12P",
  "dob": "2001-01-01"
}

GET _index_template/students_template

DELETE _index_template/students_template
```

- Elastic common schema (ECS)
	- its standardized data schema designed to provide a common set of field names and data types for logs and metrics in the Elastic stack, which includes ES, Kibana, logstash, beats and other related components.
		- Standardization
		- Flexibility
		- Interoperability
		- Ecosystem integration

- Aliases
	- In Elasticsearch, an alias is an additional name or label assigned to one or more indexes. Aliases are dynamic in nature.
	- purpose of aliases is abstraction and decoupling, blue-green deployments, filtered queries, cross-cluster search
```
PUT students/_doc/1
{
  "name": "Ram",
  "age": 20
}

PUT teachers/_doc/1
{
  "name": "Gita",
  "age": 30
}

POST _aliases
{
  "actions": [
    {
      "remove": {
        "index": "students",
        "alias": "school"
      }
    }
  ]
}

GET school/_search
{
  "query": {
    "match_all": {}
  }
}

```
- different types of actions in alias such as add, remove, filter, is_write_index etc.

**Elastic Search Query**
- One of the core functionality of elastic search is the ability to perform searches on indexed data using the \_search endpoint.
- The \_search endpoint allows you to execute complex queries against your data and retrieve relevant results efficiently.
- Query DSL (Domain Specific Language)
- Query types
	- Match query
	- Disjunction match query
	- Multi-match query
	- IDs query
	- Term query
	- Range query
	- Bool query
	- Exists query
	- Wildcard query
	- Prefix query
	- Nested query
- Match query - This query is used to perform full-text search on a field. it matches documents that contain a specific term.
	- full-text search.
	- The match query analyzes the input text and then searches for documents where the analyzed text matches the terms in the specified field.
	- relevance score measures how well each document matches a query.
```
GET students/_search
{
  "query": {
    "match": {
      "bio": "hackathons robotics data"
    }
  }
}

GET students/_search
{
  "query": {
    "match": {
      "bio": {
        "query": "cricket robotics data" # default or operator - "operator": "and"
      }
    }
  }
}

# fuzziness 
```

- Disjunction match query
	- It is useful for situations where you have multiple query clauses representing different aspects of a search, and you want to ensure that documents matching any of these clauses are returned.
	- query clauses
		- comma separated queries.
	- Tie breaker
		- it  determines how much the individual scores of the query clauses contributes to the final relevance score.

- Multi-match query
	- A versatile query that allows you to run the same query across multiple fields and combines the results.
	- Individual fields can be boosted with the (^) notation.
	- the way multi_match query is executed internally depends on the type parameter. parameters can be best_fields, cross_fields, most_fields, phrase, phrase_prefix.
	
```
GET students/_search
{
  "query": {
    "multi_match": {
      "query": "robotics coding open-source",
      "fields": ["hobbies^2", "bio"],
      "type": "best_fields" # multiple options can be there
    }
  }
}
```

- IDs query
	- This ids query allows you to specify an array of document IDs to match. It retrieves documents that have IDs that match any of the provided IDs.
	
```
GET students/_search
{
  "query": {
    "ids": {
      "values": [1, "6ghcwZQB2IGeQemkglGT"]
    }
  }
}
```

- Term Query
	- This query is used to find documents that contain a specific exact term in a particular field.
	- It's useful for searching fields that are not analyzed.
```
GET students/_search
{
  "query": {
    "match": {
      "hobbies": "coding"
    }
  }
}
```

- Range Query
	- it matches documents where the field value falls within a specified range of values.
	
```
GET students/_search
{
  "query": {
    "range": {
      "dob": {
        "gte": "2000-01-01"
      }
    }
  }
}
```

- Bool Query
	- A Boolean query that allows you to combine multiple queries using Boolean operators (must, should, must_not, filter) to create more complex queries.
```
GET students/_search
{
  "query": {
    "bool": {
      "should": [
        {
          "match": {
            "hobbies": "coding"
          }
        },
        {
          "match": {
            "bio": "engineer"
          }
        }
      ]
    }
  }
}
```
- bool query will not have relevance score since its conditional queries.

- Exists Query
	- This query matches documents where the specified field exists. Note that the field's value should have been indexed.
```
GET students/_search
{
  "query": {
    "exists": {
      "field": "hobbies"
    }
  }
}
```

- Wildcard Query
	- A wildcard Quey in  elastic search is used to search for documents where a specific field contains terms that match a specified pattern using wildcard characters. 
	- Wildcards are placeholders that allow you to match a range of terms with similar patterns.
	- * - matching any sequence of characters
	- ? - match a single character
```
GET students/_search
{
  "query": {
    "wildcard": {
      "name": "?li*"
    }
  }
}
```

- Prefix Query
	- It matches documents that have fields containing terms with a specified prefix. 
```
GET students/_search
{
  "query": {
    "prefix": {
      "name": "alice"
    }
  }
}
```

- Match Phrase Prefix Query
	- used to match start with and certain phrase
- Match Phrase Query
	- similar to above one.
- Nested Query
	- Used to query nested objects within documents.
```
GET students/_search
{
  "query": {
    "nested": {
      "path": "bag",
      "query": {
        "bool": {
          "must": [
            {
              "match": {
                "bag.name": "Laptop"
              }
            }
          ]
        }
      }
    }
  }
}
```

```
GET students/_search
{
  "_source": ["bag.name"], 
  "query": {
    "match_all": {}
  }
}
```

- Joins in Elastic Search
	- join Data Types
		- Parent Child Relationship
		- The join data types in ES allows you to model parent-child relationships between documents within the same index.
		- This is useful when you have parent child similar concept in documents.
		- its not exactly map to SQL joins but its more about structuring data and establishing relationships between documents within the same index.
		-  has_parent, has_child etc
	
```
// parent child relationships

PUT company 
{
  "mappings": {
    "properties": {
      "my_join_field": {
        "type": "join",
        "relations": {
          "dept": "emp"
        }
      }
    }
  }
}

GET company/_mapping

PUT company/_doc/2
{
  "name": "IT",
  "my_join_field": "dept"
}

PUT company/_doc/3?routing=1
{
  "name": "Ram",
  "age": 20,
  "my_join_field": {
    "name": "emp",
    "parent": 1
  }
}

PUT company/_doc/4?routing=2
{
  "name": "Shyam",
  "age": 25,
  "my_join_field": {
    "name": "emp",
    "parent": 2
  }
}

PUT company/_doc/5?routing=1
{
  "name": "Gita",
  "age": 25,
  "my_join_field": {
    "name": "emp",
    "parent": 1
  }
}

GET company/_search
{
  "query": {
    "match_all": {}
  }
}

GET company/_search
{
  "query": {
    "parent_id": {
      "id": 1,
      "type": "emp"
    }
  }
}

GET company/_search
{
  "query": {
    "has_parent": {
      "parent_type": "dept",
      "query": {
        "match": {
          "name": "HR"
        }
      }
    }
  }
}

GET company/_search
{
  "query": {
    "has_parent": {
      "parent_type": "dept",
      "query": {
        "match": {
          "name": "IT"
        }
      }
    }
  }
}


GET company/_search
{
  "query": {
    "has_child": {
      "type": "emp",
      "query": {
        "range": {
          "age": {
            "gte": 10
          }
        }
      }
    }
  }
}

GET company/_search
{
  "query": {
    "has_child": {
      "type": "emp",
      "query": {
        "match": {
          "name": "Ram"
        }
      }
    }
  }
}


GET company/_search
{
  "query": {
    "has_child": {
      "type": "emp",
      "min_children": 2, 
      "query": {
        "range": {
          "age": {
            "gte": 20
          }
        }
      }
    }
  }
}
```

```
# multi level heirarchy

DELETE company

PUT company 
{
  "mappings": {
    "properties": {
      "join_field": {
        "type": "join",
        "relations": {
          "company": ["dept", "clients"],
          "dept": "emp"
        }
      }
    }
  }  
}

PUT company/_doc/1
{
  "name": "Apple",
  "join_field": "company"
}

PUT company/_doc/1
{
  "name": "Google",
  "join_field": "company"
}

PUT company/_doc/3?routing=1
{
  "name": "IT",
  "join_field": {
    "name": "dept",
    "parent": 1
  }
}

PUT company/_doc/4?routing=1
{
  "name": "HR",
  "join_field": {
    "name": "dept",
    "parent": 2
  }
}

PUT company/_doc/5?routing=2
{
  "name": "Ram",
  "join_field": {
    "name": "emp",
    "parent": 4
  }
}

PUT company/_doc/6?routing=4
{
  "name": "Shyam",
  "join_field": {
    "name": "emp",
    "parent": 3
  }
}

GET company/_search
{
  "query": {
    "match_all": {}
  }
}

// company -> dept -> emp
// company -> clients (x)

// i want companies in which there is employees with name Ram
GET company/_search
{
  "query": {
    "has_child": {
      "type": "dept",
      "query": {
        "has_child": {
          "type": "emp",
          "query": {
            "match": {
              "name": "Ram"
            }
          }
        }
      }
    }
  }
}
```
- Limitations
	- parent and child should be in the same index and same shard
	- only one join field
	- a document can have only one parent

**Aggregation**
- Aggregation are a powerful feature in Elasticsearch that allow you to perform data analysis and extract insights from your indexed data. They are used to group, filter, and compute statistics on your data.
- ES categories aggregation in 3 categories
	1. Metrics aggregation
	2. Bucket aggregation
	3. Pipeline aggregation
- Metrics Aggregation
	- calculate metrics over a set of documents. They provide insights into the numeric values within the documents.
```
GET movies/_search
{
  "query": {
    "match_all": {}
  },
  "size": 0, 
  "aggs": {
    "averate_of_length": {
      "avg": {
        "field": "length"
      }
    },
    "sum_of_length":{
      "sum": {
        "field": "length"
      }
    },
    "min_movie_length": {
      "min": {
        "field": "length"
      }
    },
    "max_movie_length": {
      "max": {
        "field": "length"
      }
    }
  }
}
```
```
GET movies/_search
{
  "query": {
    "match_all": {}
  },
  "size": 0, 
  "aggs": {
    "distinct_directors": {
      "cardinality": {
        "field": "director.keyword"
      }
    },
    "total_movies": {
      "value_count": {
        "field": "name.keyword"
      }
    }
  }
}
```
- precision threshold can be used with cardinality.
```
GET movies/_search
{
  "query": {
    "match_all": {}
  },
  "size": 0, 
  "aggs": {
    "distinct_directors": {
      "cardinality": {
        "field": "director.keyword"
      }
    },
    "total_movies": {
      "value_count": {
        "field": "name.keyword"
      }
    },
    "stats": {
      "stats": {
        "field": "rating"
      }
    }
  }
}
```

- Bucket aggregation organize documents into buckets. or groups based on certain criteria. These criteria can be fields, ranges, date intervals, etc. They help you understand the distribution of data within various categories.
- supports nested aggregation on this type.

```
GET movies/_search
{
  "size": 0,
  "aggs": {
    "group_by_genre": {
      "terms": {
        "field": "genre.keyword",
        "min_doc_count": 0,
        "missing": "No_genre"
      },
      "aggs": {
        "my_stats": {
          "stats": {
            "field": "rating"
          }
        }
      }
    }
  }
}
```
```
GET movies/_search
{
  "size": 0,
  "aggs": {
    "my_filter": {
      "filter": {
        "term": {
          "genre.keyword": "Drama"
        }
      },
      "aggs": {
        "movie_stat": {
          "stats": {
            "field": "rating"
          }
        }
      }
    }
  }
}
```
```
GET movies/_search
{
  "size": 0,
  "aggs": {
    "plan": {
      "filters": {
        "filters": {
          "today": {
            "term": {
              "genre.keyword": "Drama"
            }
          },
          "tomorrow": {
            "term": {
              "genre.keyword": "Action"
            }
          }
        }
      },
      "aggs": {
        "names_of_movies": {
          "top_hits": {
            "size": 10,
            "_source": {
              "includes": ["name"]
            }
          }
        }
      }
    }
  }
}
```
```
GET movies/_search
{
  "size": 0,
  "aggs": {
    "group_by_year": {
      "range": {
        "field": "release_year",
        "keyed": true, 
        "ranges": [
          {
            "to": 1990,
            "key": "old-movie"
          },
          {
            "from": 1990,
            "to": 2000
          },
          {
            "from": 2000
          }
        ]
      },
      "aggs": {
        "my_stats": {
          "stats": {
            "field": "length"
          }
        }
      }
    }
  }
}
```
```
GET movies/_search
{
  "size": 0,
  "aggs": {
    "my_histogram": {
      "histogram": {
        "field": "release_year",
        "interval": 5
      },
      "aggs": {
        "movies": {
          "top_hits": {
            "size": 10,
            "_source": ["name"]
          }
        }
      }
    }
  }
}
```

**Fuzziness**
- A search technique that allows for approximate matching of terms, rather than strict exact matching.
- its useful for typos, misspelling or variations in word forms.
- fuzziness parameter controls the matching percentage by edit distance.
- Works based on edit distance algorithm
- auto parameter can be used for dynamic length

```
GET movies/_search
{
  "query": {
    "match": {
      "name": {
        "query": "night",
        "fuzziness": 2
      }
    }    
  }
}
```

