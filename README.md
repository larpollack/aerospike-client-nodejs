## Aerospike Client for node.js

The Aerospike client for node.js is an add-on module, written in C++.
 
The Aerospike node.js client supports Integer, String, Binary datatypes. 

## Prerequisites

[Node.js](http://nodejs.org) version v0.10.x is required. 
To install the latest stable version of  Node.js, visit http://nodejs.org/download/

[Node-gyp](https://github.com/TooTallNate/node-gyp) is used for building the 
library. 

The nodejs installed must be of version >= v0.10.16


Install the latest stable version of nodejs available at http://nodejs.org/download/


## Building

The aerospike-client-node.js uses aerospike-client-c library.

1. Download the C client from Aerospike website

2. Extract and go to the resulting directory.

        tar -xzvf  aerospike-client-c-<version>-<os>-x86_64.tgz
        cd aerospike-client-c-<version>-<os>-x86_64

3. Install aerospike-client-c library
    
    Centos/RHEL: 

        sudo rpm -i aerospike-client-c-devel-<version>-<os>.x86_64.rpm
        
    Debian/Ubuntu: 
    
        sudo dpkg -i aerospike-client-c-devel-<version>-<os>.x86_64.deb

4. cd to aerospike-client-node.js-0.1.0 directory.
    
        cd path/to/aerospike-client-node.js-0.1.0

5. Install `aerospike-node.js` client as a addon node_module
    
        sudo npm install -g 

6. Set the environment variable `NODE_PATH` to `/usr/lib/node_modules`.
    
        export NODE_PATH=/usr/lib/node_modules

## Examples

	Refer to examples folder which demonstrates the all the operations available in Aerospike Database.

## Sample Programming
	
	var aerospike = require('aerospike')
	var config = {
		hosts:[{ addr:"127.0.0.1", port : 3000 
			//specify the list of node ip and port in the
			//aerospike cluster, following the above format.
		    }
		    ]}
	
	// Connect call connects to the aerospike cluster.
	// Only one instance of AerospikeClient should be used across a
	// an application.	
	var client = aerospike.connect(config)

	// Error codes/ Status returned by the Aerospike Cluster for every operation
	var status = aeropsike.Status;	
	var bins = {
		a: 123,
		b: "xyz"
	}

	client.put(["test", "demo", "a"], bins, function(err, meta, key) {
	 if (err.code == status.AEROSPIKE_OK) {
	  // handle the response
	 }
	 else {
		// Error -- Record is not written to aerospike server
	 }
	})
	
	client.get(["test", "demo", "b"], function(err, bins, meta, key) {
	  if (err.code == status.AEROSPIKE_OK) {
	  	// handle the response
	  }	else {
		// Error -  Record is not retrieved
	  }
	})

	client.remove(["test","demo","a"], function(err, key) {
		//handle the response
	})

	var bins = [ 'a','b' ];
	client.select(["test","demo","a"],bins, function(err, rec, meta) {
		//handle the response 
		console.log(rec);
	})

	
	var k1 = [
         {ns:'test',set:'demo',value:"value"+1},
         {ns:'test',set:'demo',value:'value'+2},
         {ns:'test',set:'demo',value:'value'+3}]
	client.batch_get(k1,function (err, rec_list){
			if (err.code == status.AEROSPIKE_OK) {
			num_records = rec_list.length;
        	for (i=0; i<num_records; i++) {
				 if(rec_list[i].RecStatus == status.AEROSPIKE_OK) {
                	console.log(rec_list[i]);
				 }else {
					// Record was not retrieved 
				}
        	}
		}
	}

	//For graceful shutdown of Aerospike Cluster, invoke client.close()
	// when the application shuts down
	client.close();

