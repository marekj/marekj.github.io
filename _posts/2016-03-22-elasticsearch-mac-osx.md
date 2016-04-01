---
layout: post
title: First steps with Elasticsearch on Mac OS X
---

First setup your environment

## install and run

TL;DR; version. Open cli and past the following (assuming you have homebrew installed http://brew.sh/ and java)

```shell
brew update
brew install elasticsearch
brew install kibana
kibana plugin --install elastic/sense
#run it
elasticsearch -d
kibana &
open http://localhost:5601/app/sense
```


### Elasticsearch aka ES.

`brew install elasticsearch` ( I get 2.3.0 at this time)

You start with `elasticsearch` executable and default port is 9200

`elasticsearch -d` deamonize (`elasticsearch --help` to see options)

Additional executable `plugin` found in `/usr/local/Cellar/elasticsearch/2.3.0/libexec/bin` but I don't think I will install any plugins yet (see Kibana plugins instead). (info https://www.elastic.co/guide/en/elasticsearch/plugins/current/installation.html)

Conf file is in `/usr/local/etc/elasticsearch/`

Goto http://localhost:9200 and you should see a json output string with cluster_name and version info

Or in terminal `curl 'localhost:9200'`

Docs https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html

Now you have a node installed (cluster will come to mean something when you have more than one node)


### kibana

Why? To visualize your data pretty (ref: https://www.elastic.co/guide/en/kibana/current/index.html)

`brew install kibana` (I get 4.5.0 at this time)

Start in the background with `kibana &` - there is no daemonize option (you can use launchctl if you want. see instructions in `brew info kibana` how to do that. For now just put it in bg)

Starts on port 5601

Connects to default elasticserch on port 9200. See `/usr/local/etc/kibana/kibana.yml` file for conf.

Goto http://localhost:5601 to see kibana UI.

### kibana plugin Sense

Why? Web console to ES API (instead of terminal curl) (https://www.elastic.co/guide/en/sense/current/introduction.html)

You can install Sense chrome extension and you don't need kibana or add the sense plugin to your Kibana service. I got both installed so I can use Chrome extension when kibana service is not running)

`kibana plugin --install elastic/sense`

if plugin restart kibana and see it here http://localhost:5601/app/sense


## Data Concepts

In Sense app let's create an index https://www.elastic.co/guide/en/elasticsearch/reference/current/_basic_concepts.html#_index

`PUT /blabla`

`GET /_cat/indices` and you should see the one we just created `blabla`

Make a document of type `joke`

```
PUT /blabla/joke/1
{
    "title": "Once Upon A Time",
    "author": "Unknown",
    "about": "Suspension of desbelief"
}
```

You should see this document returned

```json
{
  "_index": "blabla",
  "_type": "joke",
  "_id": "1",
  "_version": 1,
  "_shards": {
    "total": 2,
    "successful": 2,
    "failed": 0
  },
  "created": true
}
```

Get the joke

`GET /blabla/joke/1`

Should return

```json
{
  "_index": "blabla",
  "_type": "joke",
  "_id": "1",
  "_version": 1,
  "found": true,
  "_source": {
    "title": "Once Upon A Time",
    "author": "Unknown",
    "about": "False and funny beliefs about times"
  }
}
```


Get the joke that's not there

`GET /blabla/joke/3`

```json
{
  "_index": "blabla",
  "_type": "joke",
  "_id": "3",
  "found": false
}
```

Delete the joke

`DELETE /blabla/joke/1`


## And now study this a bit

- Erik Rose
  - https://www.youtube.com/watch?v=lWKEphKIG8U
  - https://github.com/erikrose/elasticsearch-tutorial
