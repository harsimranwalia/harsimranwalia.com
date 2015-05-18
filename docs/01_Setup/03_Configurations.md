### Good configurations for elasticsearch nodes

**Master Node**<br>
... You can directly copy paste these into your elasticsearch.yml as well
	############# Cluster #############
	cluster.name: my_cluster

	############# Node #############
	node.name: "master"
	node.master: true
	node.data: false
	node.max_local_storage_nodes: 1
	# If you want to enable scripting for your ES, has a security risk 
	script.disable_dynamic: false

	############# Index #############
	index.number_of_shards: 4
	index.number_of_replicas: 1

	############# Memory #############
	bootstrap.mlockall: true

	############# Network And HTTP #############
	# any port you wish for es to listen on tcp, by default its 9300
	transport.tcp.port: 9300
	http.enabled: false

	############# Discovery #############
	# These setting shall work on most PaaS providers but I am using these on GCE
	discovery.zen.minimum_master_nodes: 1
	discovery.zen.ping.timeout: 10s
	discovery.zen.ping.multicast.enabled: false
	# List of master nodes
	discovery.zen.ping.unicast.hosts: ["master:9300"]

	############# Safety #############
	# Cannot delete all indices at once
	action.disable_delete_all_indices: false
	# To delete wildcard * will not work have to give complete name
	action.destructive_requires_name: true

<br>
<br>

**Client Node**<br>
... You can directly copy paste these into your elasticsearch.yml as well
	############# Cluster #############
	cluster.name: my_cluster

	############# Node #############
	node.name: "client"
	node.master: false
	node.data: false
	node.max_local_storage_nodes: 1
	# If you want to enable scripting for your ES, has a security risk 
	script.disable_dynamic: false

	############# Index #############
	index.number_of_shards: 4
	index.number_of_replicas: 1

	############# Memory #############
	bootstrap.mlockall: true

	############# Network And HTTP #############
	# any port you wish for es to listen, tcp by default 9300 and http default 9200
	transport.tcp.port: 9300
	http.port: 9200
	http.enabled: true
	# If you are planning to use kibana
	http.cors.enabled: true

	############# Discovery #############
	# These setting shall work on most PaaS providers but I am using these on GCE
	discovery.zen.minimum_master_nodes: 1
	discovery.zen.ping.timeout: 10s
	discovery.zen.ping.multicast.enabled: false
	# List of master nodes
	discovery.zen.ping.unicast.hosts: ["master:9300"]

	############# Safety #############
	# Cannot delete all indices at once
	action.disable_delete_all_indices: false
	# To delete wildcard * will not work have to give complete name
	action.destructive_requires_name: true

<br>
<br>

**Data Node**<br>
... You can directly copy paste these into your elasticsearch.yml as well
	############# Cluster #############
	cluster.name: my_cluster

	############# Node #############
	node.name: "data-1"
	# If this is your backup master node then node.master true else false
	node.master: true
	node.data: true
	node.max_local_storage_nodes: 1
	# If you want to enable scripting for your ES, has a security risk 
	script.disable_dynamic: false

	############# Index #############
	index.number_of_shards: 4
	index.number_of_replicas: 1

	############# Paths #############
	# Attach and mount an extra disk with space at /elasticsearch for data and log storage
	path.data: /elasticsearch/data
	path.logs: /elasticsearch/logs

	############# Memory #############
	bootstrap.mlockall: true

	############# Network And HTTP #############
	# any port you wish for es to listen, tcp by default 9300 and http default 9200
	transport.tcp.port: 9300
	http.port: 9200
	# If this is your backup client node then true else false
	http.enabled: true
	# If you are planning to use kibana
	http.cors.enabled: true

	############# Discovery #############
	# These setting shall work on most PaaS providers but I am using these on GCE
	discovery.zen.minimum_master_nodes: 1
	discovery.zen.ping.timeout: 10s
	discovery.zen.ping.multicast.enabled: false
	# List of master nodes
	discovery.zen.ping.unicast.hosts: ["master:9300"]

	############# Safety #############
	# Cannot delete all indices at once
	action.disable_delete_all_indices: false
	# To delete wildcard * will not work have to give complete name
	action.destructive_requires_name: true