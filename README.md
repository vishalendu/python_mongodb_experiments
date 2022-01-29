# python_mongodb_experiments
Some Python scripts related to MongoDB

Summary
--------

mongoDB-Experiments.ipynb:<br>
This script is made to demonstrate how you can use ***Python*** to understand and compare Query performance using "Explain executionStats".<br>
The script has 2 Parts:<br>
1) ***Data setup***: This part has functions that you can use to create dataset for the queries in the script<br>
2) ***Query Analysis***: This part has 2 queries that I have used to display how their performance can be identified and compared.<br><br>
  - First Query is a simple "find":<br>
    > <P>db.collection.find({"age":"20"})</P>
    If you have a mongoDB CLI or you can use any client like Robo 3T. You can get the Explain plan with executionStats using the following:<br>
    > <P>db.collection.find({"age":"20"}).explain('executionStats')</P>
      ***Test Cases:*** <br>
      Try1: Without any index<br>
      Try2: With index on "age" <br>
      
  - Second Query is an aggregate query:<br>
    > <P>db.collection.aggregate([{"$match":{"$and":[{"age":{"$lte":20}},{"gender":{"$eq":"Male"}}]}}])</P>
    This is a simple single stage aggregate with a 2 predicates to look at a compound index. If you want to check out its Explain plan with executionState:<br>
    > <P>db.collection.explain('executionStats').aggregate([{"$match":{"$and":[{"age":{"$lte":20}},{"gender":{"$eq":"Male"}}]}}])</P>
      ***Test Cases:***<br>
      Try1: Without any index<br>
      Try2: With index on age<br>
      Try3: With compound index on age,gender
<br><br>


Addendum
---------
If you dont have a mongoDB instance, you can easily set one up using docker. Here is a simple command you can use to start a mongoDB container:
I have used mongo:4.4.12 for this experiment since I setup mongoDB on my RaspberryPi and this version is the last supported version on the platform. You can use the latest version of mongoDB as well.<br>
*Just remember to create a ```datadir``` directory in the directory where you run this command. That will store all the datafiles.*
```
docker run -d --name some-mongo \
    -p 27017:27017 \
    -v `pwd`/datadir:/data/db \
    -e MONGO_INITDB_ROOT_USERNAME=mongoadmin \
    -e MONGO_INITDB_ROOT_PASSWORD=secret \
    mongo:4.4.12
```
This will create a default admin user ```mongodbadmin``` with password ```secret``` <br>
Your connection string, if you run this on your local system will be : ```mongodb://mongoadmin:secret@localhost:27017```

<br><br>

References:
---------
https://docs.mongodb.com/manual/reference/explain-results/
