# Elastic Blog:Elasticsearch Basic Concepts
https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html

- A `cluster` is a collection of one or more nodes (servers) that together holds your entire data and provides federated indexing and search capabilities across all nodes.
- A `node` is a single server that is part of your cluster, stores your data, and participates in the cluster’s indexing and search capabilities.
- An `index` is a collection of documents that have somewhat similar characteristics.
  - In a single cluster, you can define as many indexes as you want.
- Within an index, you can define one or more types. A `type` is a logical category/partition of your index whose semantics is completely up to you. In general, a type is defined for documents that have a set of common fields.
- A `document` is a basic unit of information that can be indexed. 
  - Within an index/type, you can store as many documents as you want.
- Elasticsearch provides the ability to subdivide your index into multiple pieces called shards. `Sharding` is important for two primary reasons:
  - It allows you to horizontally split/scale your content volume.
  - It allows you to distribute and parallelize operations across shards (potentially on multiple nodes) thus increasing performance/throughput.
- Elasticsearch allows you to make one or more copies of your index’s shards into what are called replica shards, or replicas for short. `Replication` is important for two primary reasons:
  - It provides high availability in case a shard/node fails. For this reason, it is important to note that a replica shard is never allocated on the same node as the original/primary shard that it was copied from.
  - It allows you to scale out your search volume/throughput since searches can be executed on all replicas in parallel.
- Once replicated, each index will have `primary shards` (the original shards that were replicated from) and `replica shards` (the copies of the primary shards).
