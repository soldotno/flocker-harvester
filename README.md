# Flocker setup for MongoDB, Elasticsearch, Kibana, Logstash

### Requirements

Vagrant (1.6.2 or newer)
VirtualBox


### Getting up and running


```bash 
git clone git@github.com:soldotno/flocker-harvester.git
cd !$
vagrant up
flocker-deploy 172.16.255.250 elk-deployment.yml elk-application.yml
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

Test that it works:
```bash curl -XGET 'http://127.0.0.1:9200/harvester/_search?q=Poseforbruket&pretty' ```

**MongoDB River Administration** 
http://127.0.0.1:9200/_plugin/river-mongodb/



