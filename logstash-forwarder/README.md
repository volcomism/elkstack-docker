Docker Logstash Forwarder
=========================

Docker image for [Logstash
Forwarder](https://github.com/elasticsearch/logstash-forwarder), formerly known
as _lumberjack_.

Prerequisites
-------------

In order to use this image, you MUST [create an SSL
certificate](https://github.com/elasticsearch/logstash-forwarder#generating-an-ssl-certificate),
and [configure Logstash
Forwarder](https://github.com/elasticsearch/logstash-forwarder#configuring)
using a `config.json` file. This configuration file MUST be named `config.json`
and MUST be located in `/etc/logstash-forwarder`.

### SSL Certificate

If you want to generate self-signed SSL certificates and use an IP address
rather than a DNS record to point to your `logstash` server(s), then you SHOULD
use this
[lc-tlscert](https://github.com/driskell/log-courier/blob/master/src/lc-tlscert/lc-tlscert.go)
tool:

```
$ wget https://raw.githubusercontent.com/driskell/log-courier/master/src/lc-tlscert/lc-tlscert.go
$ go run lc-tlscert.go
```

Copy the generated `selfsigned.{crt,key}` files to the `logstash-forwarder`
server **and** to the `logstash` server.

### Logstash Forwarder Configuration

Below is a basic configuration for Logstash Forwarder:

``` json
{
  "network": {
    "servers": [ "prod-datalake-control-01.datalake.chip-ec2.net:5001" ],
    "ssl ca": "/etc/ssl/logstash.crt"
  },
  "files": [
    {
      "paths":  [ "/var/log/syslog" ],
      "fields": { "type": "syslog" }
    }
  ]
}
```

Usage
-----


## Using Docker Compose

``` yaml
logstashforwarder:
  image: chip/logstash-forwarder
  volumes:
    - ./config.json:/etc/logstash-forwarder/config.json
    - ./logstash.crt:/etc/ssl/logstash.crt
    - ./logstash.key:/etc/ssl/logstash.key
  volumes_from:
    - logs

logs:
  image: busybox
  volumes:
    - /var/log/syslog:/var/log/syslog

```
