### System requirements

* Data in mongodb (Replica set mandatory)
* Elasticsearch
* [Sense](https://chrome.google.com/webstore/detail/sense-beta/lhjgkmllcaadmopgmanpapmpjgmfcfig)
	+ for making API requests to elasticsearch

#### Reference
http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/search-suggesters-completion.html

### Steps to perform

Lets create an autocomplete of location for the ease of this example with the following names(can change them according to your choice)

		* Only for reference
		index_name = location
		mapping_name = autocomplete
		river_name = location

#####**1. Create a new index**
... The put request is used to create a new index in elasticsearch

		PUT /location

#####**2. Create the mapping for the index**
... This put request specifies the mapping defined in the json

... Mapping defines that we should create a completion suggestor from the name field of current_location object

... We have used [simple analyzer](http://www.elasticsearch.org/guide/en/elasticsearch/reference/current/analysis-simple-analyzer.html). You can use whichever suits your requirement.The *max_input_length* defines the maximum number of charactes to break the word into. For eg. there is a word that is 40 characters long, it will only support the autocompletion till the first 30 characters as specified by us.
	
		PUT /location/_mapping/autocomplete
		{
			"properties": {
				"current_location": {
					"type": "object",
					"properties": {
						"name": {
							"type": "completion",
							"index_analyzer": "simple",
							"search_analyzer": "simple",
							"max_input_length": 30
						}
					}
				}
			}
		}

#####**3. Create the river**
... We now create the river through which the data from mongodb will flow into elasticsearch for indexing.

... This river while running will keep all the data updated in ES as its updated in mongodb. 

... Mention the ip address and port combination of our mongodb Replica set members (excluding arbiter)

		POST /_river/location/_meta
		{
			 "type": "mongodb",
			 "mongodb": {
					"servers": [
						 {
								"host": "x.y.z.a",
								"port": 27017
						 },
						 {
								 "host": "a.b.c.d",
								 "port": 27017
						 }
					],
					"options": {
						"secondary_read_preference": true,
						"include_fields": ["current_location"]
					},
					"db": "amoeba",
					"collection": "profiles"
			 },
			 "index": {
					"name": "location",
					"type": "autocomplete"
			 }
		}

... The *include_fields* option helps us to pick and choose the only fields that should be indexed from the document (which could have other fields)

#### **Test**
... Now that we have implemented autocomplete, lets test it out!

		POST /location/_suggest?pretty
		{
				"location-suggest" : {
						"text" : "th",
						"completion" : {
								"field" : "current_location.name",
								"size": 10
						}
				}
		}


