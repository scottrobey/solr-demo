# solr-demo
Demonstrates building and querying a sharded Solr index on multiple VMs

## Setup
Download Solr Server tar ball to project root directory

Example: `wget https://www.apache.org/dyn/closer.lua/lucene/solr/8.1.1/solr-8.1.1.tgz`


## Requirements

* Virtual Box
* Vagrant
* JDK11
* Gradle (4.x)
* Solr Server - https://lucene.apache.org/solr/downloads.html

## Reference

* https://lucene.apache.org/solr/guide/6_6/distributed-search-with-index-sharding.html
* https://lucene.apache.org/solr/guide/6_6/index-replication.html
* https://wiki.apache.org/solr/SolrPerformanceProblems
* https://wiki.apache.org/solr/SolrPerformanceFactors

Misc & SO

* https://stackoverflow.com/questions/27637174/solr-sharding-on-same-machine-useful-or-not

Enable syntax highlighting for Vagrantfile:
File Types -> Ruby -> Registered Patterns -> Add "Vagrantfile"
