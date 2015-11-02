# Docker ELK stack

Run the ELK (Elasticseach, Logstash, Kibana) stack with Docker and Docker-compose.

It will give you the ability to quickly test your logstash filters and check how the data can be processed in Kibana.

Based on the official images:

* [elasticsearch](https://registry.hub.docker.com/_/elasticsearch/)
* [logstash](https://registry.hub.docker.com/_/logstash/)
* [kibana](https://registry.hub.docker.com/_/kibana/)

# HOW TO

## Setup

1. Install [Docker](http://docker.io).
2. Install [Docker-compose](http://docs.docker.com/compose/install/).
3. Clone this repository

### Start the stack and inject logs

First step, you can edit the logstash-configuration in *logstash-conf/logstash.conf*. You can add filters you want to test for example.

Then, start the ELK stack using *docker-compose*:

```
$ docker-compose up -d
```

Now that the stack is running, you'll want to inject logs in it. The shipped logstash configuration allows you to send content via tcp:

```
$ nc localhost 5000 < /var/log/syslog
```


### Playing with the stack

The stack exposes 4 ports on your localhost:

* 5000: Logstash TCP input.
* 9200: Elasticsearch HTTP (with Marvel plugin accessible via [http://localhost:9200/_plugin/marvel](http://localhost:9200/_plugin/marvel))
* 5601: Kibana 4 web interface (8080 on host), access it via [http://localhost:8080](http://localhost:8080)
