# Logstash
Logstash tool to get and parse all the logs sent via NXLog.
* [Logstash](https://www.elastic.co/products/logstash) - Tool used


## Getting Started

First download or copy this repository in order to install logstash
We are going to need a file to update the configuration we want to use,
in this case, get all logs via tcp and parse in .json

* [Logstash Config example](https://www.elastic.co/guide/en/logstash/current/configuration.html) - Example


## Configuration

Create a new file to upload from there the config to elastic.
Get via tcp and default port the logs from host, (VM W7 machine).

```
input {
  tcp {
    codec => json
    port => 5044
    host => "192.168.56.1"
  }
}
```

Get everything to forward to elastic with the panopticon file as the default config
```
output {
  elasticsearch {
  hosts => ["http://192.168.56.1:9200/"]
  index	=> "logstash-%{+YYYY.MM.dd}"
  template => "/home/cuckoo/logstash/panopticon_mapping.json"
  template_overwrite => true
  }
```

Last create the mapping for your logs in the file defined to get to elastic.
```
"mappings" : {
    "_default_" : {
    (queries...)
```

Important! 
Setting shards to 1 to not to collapse ram in the device used, if want to use more, use distributed.
(also changing the config in ElasticSearch)
```
"template" : "logstash-*",
  "settings" : {
    "number_of_shards": 1,
    "number_of_replicas":0,
```
