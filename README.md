# Flocker setup for MongoDB, Elasticsearch, Kibana, Logstash

## Objectives

Getting up and running with MongoDB and Elasticsearch with minimal fuzz.


### Requirements

- Vagrant (1.6.2 or newer)
- VirtualBox


### Start it


```bash
git clone git@github.com:soldotno/flocker-harvester.git
cd !$
vagrant up
flocker-deploy 172.16.255.250 deployment.yml application.yml
```

Now you should be able to access MongoDB

```bash
mongo 172.16.255.251
```

Logstash: http://172.16.255.250/



## Wishlist

### Install the plugin that connects MongoDB with Elasticsearch

```bash
`brew list elasticsearch | ack plugin` --install com.github.richardwilly98.elasticsearch/elasticsearch-river-mongodb/2.0.9
`brew list elasticsearch | ack plugin` --install elasticsearch/elasticsearch-mapper-attachments/2.5.0
```

Notify ES about the mongodb river

```bash
$ curl -XPUT 'http://172.16.255.250:9200/_river/mongodb/_meta' -d @init.json
```

**init.json**

```json
{
 "type": "mongodb",
    "mongodb": {
      "db": "harvester",
      "collection": "entries"
    },
    "index": {
      "name": "harvester",
      "type": "entries"
    }
}
```

Do a wildcard search:

```bash
curl -XGET 'http://172.16.255.250:9200/harvester/_search?pretty=true&q=*:*'
{
  "took" : 1,
  "timed_out" : false,
  "_shards" : {
    "total" : 5,
    "successful" : 5,
    "failed" : 0
  },
  "hits" : {
    "total" : 0,
    "max_score" : null,
    "hits" : [ ]
  }
}
```

**MongoDB River Administration**
http://172.16.255.250:9200/_plugin/river-mongodb/



