### Some terminologies explained
______

**Elasticsearch Cluster?**<br>
... It is a set of elasticsearch nodes running in conjunction with each other

**Elasticsearch Node?**<br>
... It is a single instance of VM running elasticsearch

**Different types of Nodes?**<br>
* Master<br>
	... These nodes are the ones with store all the configurations and maintain the live state of the cluster
* Client<br>
	... These nodes act as smart load balancers and used as http endpoint for rest API calls
* Data<br>
	... These are dedicated data nodes which are responsible for storing data and serving requests 

<br>
### Best practices setup for an Elasticsearch Cluster?
______
#####Should have the following set of ES nodes with minimum resource requirements*<br>
* Atleast 1 master node
	* 1 core CPU, 4 GB Memory
* Atleast 1 client node
	* 2 core CPU, 8 GB Memory
* 1 or 2 data nodes
	* 4 core CPU, Memory calculated as (Total Index size/4) GB each node

#####Sample configuration that I use on GCE (Google Compute Engine)
* 1 dedicated master node
	* n1-standard-1 machine
* 1 client node
	* n1-standard-1 machine
* 2 data nodes
	* n1-highmem-4 and n1-highmem-8 machine
	* 1 data node acting as a backup master and backup client node


######*Minimum resource requirement based on production grade cluster, your cluster may run on lower resources
