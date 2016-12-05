********MVCC Based Distributed Policy Evaluation Engine********************************

This is a da(python) based implementation of horizontally scaled distributed
policy evaluation engine.
****************************************************************************

INSTRUCTIONS - HOW TO RUN?
1.	Type make in the main directory to build the code.

2.	Type make ARG=(NAME_OF_CONFIG_FILE) run to run the code with a specific
	configuration. This configuration is specified in a config file which
	is supplied the placeholder NAME_OF_CONFIG_FILE. There is one sample
	config file in config directory. There are many test cases config files

MAIN FILES:
1. 	client.da - Process to send request and receive a response. This process directly
	sends the rquest to subject coordianator and receives a evaluation response
	to its request

2.	coordinator.da - Coordinator process can be a subject coordinator or resource 
	coordinator. The functionality is decided upon the source from where the
	message is received (receive tag). Appropriate processing is done on each
	stage. 

	Subject coordinator receives request from client, forwards it to 
	resource coordinator, receives response from worker, resolves subject
	conflict if any and passes result to client

	Resource cooridnator forwards request to worker, resolves conflicts and
	alerts subject coordinator if there are any conflicts with respect to the
	resource attribute upates

	Both the cooridnators maintain caches, subject cooridnator maintains an 
	additional tentative cache. Both coordinators fire updates to the database.

3. 	worker.da - Reads policy.xml file. Receives request, retrieves the needed 
	attributes from database and applies the policy rules on the request.
	It sends the result of evalution to coordinator, the attributes to be 
	updated if any and the attributes read.

4.  database.da - Initializes an in memory database from a init xml file.
	It serves the queries issued by worker with a response that contains values 
	for the asked attributes for a given entity(subject or resource)

5.  master.da- Responsible to receive completion alerts from the clients.
	For simplicity of this project, all the processes are initialized from the
	single main funciton in master.da file

6. common.da - Definitions of datastructures like Requst, Response, Database
	response


ASSUMTIONS
1.	Every subject and resource has a unique id which serves
	as the primary key to identify it in database as well. This id represents
	each entity(subject or object) and is supplied in request.

BUGS / LIMITATIONS - None

CONTRIBUTIONS - 
	SHAHZEB PATEL:
	AADARSH JAJODIA

	All tasks done together. So no specific distinguising. Same efforts.
	
OTHER COMMENTS:
	Every single detail of the process (what's happening is logged). This can be seen
	by switching to DEBUG log in the config file. Else minimal logging is turned on
	by default