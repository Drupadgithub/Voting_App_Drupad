# Voting_App_Drupad
Voting App 



Diagram:
 
Steps Followed:

Step 1:   Clone voting app.
Step 2:   Go to K8’s specification and apply yaml file.
 Change the result service and vote service port for worker nodes.
 

 

Change names:
 

Go to root user using “sudo su” and the path where voting app is cloned. Then apply yaml files in K8’s-specification directory using command “kubectl apply -f . -n <any namespace> 

 

Now pods are created under ns provided in previous steps. Check using command “kubectl get pods -n <namespace>
 

Perform kubectl get all -n <namespace> to know all service, pod , replicaset and deployment details.
 
Here node port is assigned for voting and result pods as shown in above snippet.

Step 3: Open Browsers for voting and result. Vote and check result
 

 

Step 4 : Delete Vote Pod
Deleted the pod using command “ kubectl delete pod vote-94849dc97-svvfq -n drup-kumar”
 

Observation: On Unix vote Pod got terminated and a new pod is created. Shown below in snippet.

 
On Browser, no change in result page and voting page. In voting page on refresh just the tick mark was disappeared


Voted again and checked. Its reflecting in results.

Step 5: Delete Worker Pod
Delete worker pod using command “kubectl delete pod worker-dd46d7584-gm4z8 -n drup-kumar”
 

Observation: Pod got deleted and new pod got added. No change in voting and result page. As shown in below snippet.
 

 

 

Try voting after restart.
Its working.
 
 

Step 6: Delete DB pod.
Delete DB pod using command “kubectl delete pod db-b54cd94f4-sl8gr -n drup-kumar”
 

Observation: Db pod got deleted and recreated. Observed that result page shows no votes yet. All votes are deleted or reset. Shown in below snippets
 

 

 

Tried Voting again. It works.
 


 



Queries: 
1.	We can provide only 2 votes. Not able to vote more than 2. Why is it so?
2.	I tried changing name from cats, dogs to something else but same is not reflected in Gui. I changed in all files in results/views and vote/app.py files.
3.	I did not observe issue of result pod not working after db pod restart. Why?
Below is the logs from new DB pod.

[root@ip-172-31-42-42 ~]# kubectl logs db-b54cd94f4-l7w2g -n drup-kumar
The files belonging to this database system will be owned by user "postgres".
This user must also own the server process.

The database cluster will be initialized with locale "en_US.utf8".
The default database encoding has accordingly been set to "UTF8".
The default text search configuration will be set to "english".

Data page checksums are disabled.

fixing permissions on existing directory /var/lib/postgresql/data ... ok
creating subdirectories ... ok
selecting default max_connections ... 100
selecting default shared_buffers ... 128MB
creating configuration files ... ok
creating template1 database in /var/lib/postgresql/data/base/1 ... ok
initializing pg_authid ... ok
setting password ... ok
initializing dependencies ... ok
creating system views ... ok
loading system objects' descriptions ... ok
creating collations ... ok
creating conversions ... ok
creating dictionaries ... ok
setting privileges on built-in objects ... ok
creating information schema ... ok
loading PL/pgSQL server-side language ... ok
vacuuming database template1 ... ok
copying template1 to template0 ... ok
copying template1 to postgres ... ok
syncing data to disk ... ok

WARNING: enabling "trust" authentication for local connections
You can change this by editing pg_hba.conf or using the option -A, or
--auth-local and --auth-host, the next time you run initdb.

Success. You can now start the database server using:

    postgres -D /var/lib/postgresql/data
or
    pg_ctl -D /var/lib/postgresql/data -l logfile start

****************************************************
WARNING: No password has been set for the database.
         This will allow anyone with access to the
         Postgres port to access your database. In
         Docker's default configuration, this is
         effectively any other container on the same
         system.

         Use "-e POSTGRES_PASSWORD=password" to set
         it in "docker run".
****************************************************
waiting for server to start....LOG:  database system was shut down at 2023-07-21 11:53:55 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
 done
server started

/usr/local/bin/docker-entrypoint.sh: ignoring /docker-entrypoint-initdb.d/*

LOG:  received fast shutdown request
LOG:  aborting any active transactions
LOG:  autovacuum launcher shutting down
LOG:  shutting down
waiting for server to shut down....LOG:  database system is shut down
 done
server stopped

PostgreSQL init process complete; ready for start up.

LOG:  database system was shut down at 2023-07-21 11:53:56 UTC
LOG:  MultiXact member wraparound protections are now enabled
LOG:  database system is ready to accept connections
LOG:  autovacuum launcher started
ERROR:  relation "votes" does not exist at character 38
STATEMENT:  SELECT vote, COUNT(id) AS count FROM votes GROUP BY vote
ERROR:  relation "votes" does not exist at character 38
STATEMENT:  SELECT vote, COUNT(id) AS count FROM votes GROUP BY vote
ERROR:  relation "votes" does not exist at character 38
STATEMENT:  SELECT vote, COUNT(id) AS count FROM votes GROUP BY vote
[root@ip-172-31-42-42 ~]#

 
Learnings:
1.	Docker concept
2.	Kubernetes architecture and concept
3.	Basic understanding of kube commands 
4.	Using Kubernetes in daily work
5.	Pods, cluster, worker node , master node  concept


 


 

I have sent the documents(which has screenshots) over email also i.e kalyanaraman.m@zekelabs.com 
