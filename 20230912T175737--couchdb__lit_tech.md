---
title:      "couchdb"
date:       2023-09-12T17:57:37-04:00
tags:       ["lit", "tech"]
identifier: "20230912T175737"
---

Cluster of unreliable commodity hardware = COUCH

Horizontal scalability 
- add extra node horizontally 

Vertical scalability
- more ram and disk

can be run on low level hardware like rasperby pies

NoSQL

Schemaless

REST API

MapReduce in javascript

``` javascript
"dead": {
    // map by year of death
	"map": "function(doc){
	  if(doc.year_death != null){
		  emit(doc.year_death, doc.stagename);
	  }
	}"
}

"nationality": {
    // map by nationality first
	"map": "function(doc){
		emit(doc.nationality, 1);
	}",
	// then count them
	"reduce": "_count",
}
```

Eventual consistency

Couch Replication Protocol
- synchronizing over http
- rest API

http://localhost:5984/_utils/ :: login
- no more futon

http://127.0.0.1:5984/_utils/#/setup
- you need to run this to setup

view is like a query

# mango query #

``` json
{
	"selector": {
		"year_death": {
			"$gt": null
		}
	},
	"fields": ["year_death","stagename","name"],
	"limit": 3,
	"skip": 0
}
```

Multi-version concurrency control (MVCC) instead of locks
- instead of locking a document, an old version is always available for reading.

you upload a document instead of inserting values

Design documents with MapReduce instead of SQL

# CAP Theorem #

Consistency
- ensuring the most recent is retrieved

Availability
- ensuring that a document can be retrieved immediately
  even if not the most recent
  
Partition Tolerance
- can talerate is one of the node go down.  it will keep working

# Replication #
Master-Master replication 
- any node can make changes 

since couch uses a rest api there is no need to have a specialize library.

REST API
========

create database
`put http://127.0.0.1/<db-name>`

delete database
`delete http://127.0.0.1/<db-name>`

get database
`get http://127.0.0.1/<db-name>`

create document
```
POST http://127.0.0.1/<db-name>
body:
  { "notes": "foobar" }
```


update document
```
POST http://127.0.0.1/<db-name>
body:
  { "_id":...,  "_rev":...., "notes":"this was changed"}
```

delete document
```
DELETE http://127.0.0.1/<db-name>/<_id>?rev=<_rev>
```

get document
```
GET http://127.0.0.1/<db-name>/<_id>
```
- you can also pass in rev, to get a specific revision

add an attachment to a document
```
PUT http://127.0.0.1/<db-notes>/<_id>/<atachment>?rev=
headers:
  Content-Type: <image/jpeg>
```


get attachemnts
```
GET http://127.0.0.1/<db>/<_id>?attachments=true
```


get all docs
`GET http://127.0.0.1/<db>/_all_docs`

update or delete docs
`POST http://127.0.0.1/<db>/_build_docs`


Design Documents
================

they are the main query system for couchdb

they use Map Reduce to query data

show all the ones who died
``` javascript
"dead": {
	"map": "function(doc) {
		if(doc.year_death != null){
		  emit(doc.year_death, doc.stagename);
	    }"
	}"
}
```

count nationality
``` javascript
"nationality": {
	"reduce": "_count",
	"map": "function(doc){
		emit(doc.nationality, 1)
	}"
}
```

# types of design documents #

view ::
emits a key value pairs

show ::
takes a single document and displays it is some way, like HTML

list ::
shows multiple documents

update ::
make broad changes across a database

validation ::
called every time you add a document,
to validate it

sum all the docs
``` json
{
  "_id": "...",
  "_rev": "...", 
  "views": {
    "newview": {
	  "map": "function(doc) { emit(doc._id, 1);}",
	  "reduce": "_sum"
	}
  },
  "language": "javascript"
}
```

to call a design document
`http://my-fauxton:5984/<database>/_design/<design_doc>/_views/<view>`

views create BTree structures

``` json
"nationality": {
  "map":"function(doc){
	emit(doc.nationality, 1);
  }",
  "reduce": "_count"
}
```



