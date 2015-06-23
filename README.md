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
