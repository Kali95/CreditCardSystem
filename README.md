# Core Java
•	Project name is CreditCardSystem.
•	Open project in your IDE and be sure to change the database parameters.
o	In database package go to DatabaseConnection.java file and change database name as well as username and password if needed.
•	Clean and Build project
•	Run myquery.java file
RDBMS/mySQL Description :
•	To create tables and insert data for this project, run following mySQL file: 
o	CDW_SAPP_CDW_SAPP_BRANCH
o	CDW_SAPP_CDW_SAPP_CREDIITCARD
o	CDW_SAPP_CDW_SAPP_CUSTOMER
•	This file will create database for you named CDW_SAPP
Hadoop/hdfs/dataware housing
•	In HDFS we are using maria_dev user. Therefore data transferred from relational database with Sqoop is stored in /user/maria_dev/Credit_Card_System
Hive and Partition
•	Hive imports consist of four tables (Branch, Credit Card, Time and Customer). Each table is external table that loads data files imported by sqoop.
•	CreateBranchTable.sql
o	This file creates Branch tables called CDW_SAPP_D_BRANCH
o	 Loads data from /user/maria_dev/Credit_Card_System/Branch
•	CreateCreditCardTable.sql
o	This file creates CreditCrad tables called CDW_SAPP_F_CREDITCARD
o	Load data from/user/maria_dev/Credit_Card_System/CreditCard
•	CreateCustomerTable.sql
o	This file creates Customer tables called CDW_SAPP_D_CUSTOMER
o	Loads data from /user/maria_dev/Credit_Card_System/Customer
•	CreateTimeTable.sql
o	 This file creates Time tables called CDW_SAPP_D_TIME
o	 Loads data from /user/maria_dev/Credit_Card_System/TimeID
•	Incrementals.sql
o	 This file insert new data in incremental way.
o	 It is used in oozie with coordinators for incremental update.
Oozie (Sqoop and Hive)
•	Before running any sqoop jobs it is important to run sqoop metastore.
•	In /SqoopImport directory there is file called sqoopjobs.sh which is shell script that will create all Sqoop jobs.
•	Transfer sqoopjobs.sh file to your local path
•	Run following command as root user: root 
/user/maria_dev/sqoopjobs.sh
•	Run it: ./sqoopjobs.sh
•	Transfer directory /OozieWorkflow and /HiveImports to both local and hdfs path:
o	
	Type: hadoop fs --put OozieWorkflow/ /user/maria_dev/
	Type: hadoop fs --put HiveImports/ /user/maria_dev/
•	--Upload java-json.jar file:
o	oo There will be file java-json.jar
o	oo Use Ambari to upload that file to /user/oozie/share/lib/ lib_******* /sqoop/
o	oo Change lib_******* to your directory name
•	--Before running Oozie import mysql database required for this project. File to insert is in /mySQL/CDW_SAPP.sql
•	Run this command to start Oozie to create Hive tables and import data from relational database:
o	oo oozie job --oozie http://localhost:11000/oozie -config /home/maria_dev/OozieWorkflow/Initialize/job.properties -run
Oozie (Sqoop and Hive optimized)
•	After Initialized Oozie workflow, we can run Incremental Oozie workflow for incremental update
•	We are running same sqoop jobs and one additional hive query for updating data with dynamic partitioning.
•	To run optimized oozie run:
o	oo oozie job --oozie http://localhost:11000/oozie -config /home/maria_dev/OozieWorkflow/Incremental/job.properties -run
Visualization
•	In /HiveVisualization directory there are two hive querys.

