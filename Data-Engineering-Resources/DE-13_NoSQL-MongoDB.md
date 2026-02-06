## Starting mongodb server on windows CMD

#### very important Note: click right mouse button to paste command in shell 
#### checking mongodb installed in system or not
    mongod 

#### checking mongod and mongosh version
    mongod --version
    mongosh --version

#### Starting mongodb server in shell 
    mongosh

#### List databases in mongodb
    show dbs

#### Creating database named training, If a database named training already exists it will start using it
    use training

#### creating a collection name mycollection inside training database
    db.createCollection("mycollection")

#### List all collections
    show collections

#### Inserting documents into mycollection collection, below command inserts json document {"color":"white","example":"milk"} into mycollection
    db.mycollection.insert({"color":"white","example":"milk"})
    db.mycollection.insert({"color":"blue","example":"sky"})

#### Counting number of documents in a collection, first one is old command, second one is new command and it is recommended to use
    db.mycollection.count() 
    db.mycollection.countDocuments()

#### Listing all documents in a collection, mongodb automatically adds an _id field to every document in order to uniquely identify document
    db.mycollection.find()

#### Disconnecting from mongodb server
    exit

## MongoDB CRUD

#### using training database and creating collection named languages, after that Inserting documents into collection languages
    use training
    db.createCollection("languages")
    db.languages.insert({"name":"java","type":"object oriented"})
    db.languages.insert({"name":"python","type":"general purpose"})
    db.languages.insert({"name":"scala","type":"functional"})
    db.languages.insert({"name":"c","type":"procedural"})
    db.languages.insert({"name":"c++","type":"object oriented"})

#### Counting all documents in languages collection
    db.languages.countDocuments()

#### List all documents in collection
    db.languages.find()

#### List first 3 documents in collection
    db.languages.find().limit(3)

#### Query for python in languages collection
    db.languages.find({"name":"python"})

#### Query for object oriented in collection languages
    db.languages.find({"type":"object oriented"})

#### List only on specific fields, can specify what fields to see or skip in output using a projection document
#### command below lists all documents with only name field in output
    db.languages.find({},{"name":1})

#### command below lists all documents without name field in output
    db.languages.find({},{"name":0})

#### command below lists all object oriented languages with only name field in output
    db.languages.find({"type":"object oriented"},{"name":1})

#### Updating documents
- Adding a field to all documents in languages collection, updateMany command is used to update documents in a mongodb collection and it has following generic syntax: db.collection.updateMany({what documents to find},{$set:{what fields to set}})

#### adding a field description with value programming language to all documents
    db.languages.updateMany({},{$set:{"description":"programming language"}})

#### Setting a field named creator for python language
    db.languages.updateMany({"name":"python"},{$set:{"creator":"Guido van Rossum"}})

#### Setting a field named compiled with a value true for all object oriented languages, after that Listing all documents to view changes made to them
    db.languages.updateMany({"type":"object oriented"},{$set:{"compiled":true}})
    db.languages.find()

#### Delete documents based on a criteria
#### Deleting scala language document, first one is old command, second one is new command and it is recommended to use
    db.languages.remove({"name":"scala"})
    db.languages.deleteOne({"name":"scala"})

#### Deleting object oriented document in languages collection
    db.languages.deleteOne({"type":"object oriented"})

#### Delete all documents in a collection, first one is old command, second one is new command and it is recommended to use
    db.languages.remove({})
    db.languages.deleteMany({})

## MongoDB Indexing

#### Creating a collection named bigdata
    db.createCollection("bigdata")

#### code given below will insert 10000 documents into bigdata collection, Each document would have a field named account_no which is a simple auto increment number And a field named balance which is a randomly generated number to simulate bank balance for account
    for (i=1;i<=10000;i++){print(i);db.bigdata.insert({"account_no":i,"balance":Math.round(Math.random()*1000000)})}

#### Verify 10000 documents got inserted or not by count all documents
    db.bigdata.countDocuments()

#### Measuring time taken by a query by using index
#### query to find details of account number 5898, explain function used to find time taken to run query in milliseconds
    db.bigdata.find({"account_no":5898}).explain("executionStats").executionStats.executionTimeMillis

#### creating an index on field account_no, usually field that used for most query are used as an index 
    db.bigdata.createIndex({"account_no":1})

#### list of indexes on bigdata collection, should see an index named account_no_1 in output
    db.bigdata.getIndexes()

#### running a query to find how much time it takes to complete using an index, query for details of account number 9271
    db.bigdata.find({"account_no": 9271}).explain("executionStats").executionStats.executionTimeMillis

#### Deleting an index
    db.bigdata.dropIndex({"account_no":1})

#### creating an index on balance field
    db.bigdata.createIndex({"balance":1})

#### Query for documents with a balance of 10000 and find time taken to run query in milliseconds
    db.bigdata.find({"balance":10000}).explain("executionStats").executionStats.executionTimeMillis

## MongoDB Aggregation

#### Creating a collection named marks
    db.createCollection("marks")

#### Loading sample data into marks collection in training database
    db.marks.insert({"name":"Ramesh","subject":"maths","marks":87})
    db.marks.insert({"name":"Ramesh","subject":"english","marks":59})
    db.marks.insert({"name":"Ramesh","subject":"science","marks":77})
    db.marks.insert({"name":"Rav","subject":"maths","marks":62})
    db.marks.insert({"name":"Rav","subject":"english","marks":83})
    db.marks.insert({"name":"Rav","subject":"science","marks":71})
    db.marks.insert({"name":"Alison","subject":"maths","marks":84})
    db.marks.insert({"name":"Alison","subject":"english","marks":82})
    db.marks.insert({"name":"Alison","subject":"science","marks":86})
    db.marks.insert({"name":"Steve","subject":"maths","marks":81})
    db.marks.insert({"name":"Steve","subject":"english","marks":89})
    db.marks.insert({"name":"Steve","subject":"science","marks":77})
    db.marks.insert({"name":"Jan","subject":"english","marks":0,"reason":"absent"})

#### Limiting rows in output, $limit operator can limit number of documents printed in output
    db.marks.aggregate([{"$limit":2}])

#### Sorting based on a column, commands below sorts documents based on field marks in ascending order and then descending order
    db.marks.aggregate([{"$sort":{"marks":1}}])
    db.marks.aggregate([{"$sort":{"marks":-1}}])

#### Sorting and limiting
#### Aggregation usually involves using more than one operator, A pipeline consists of one or more operators declared inside an array, operators are comma separated, Mongodb executes first operator in pipeline and sends its output to next operator
#### creating a two stage pipeline that shows top 2 marks
    db.marks.aggregate([{"$sort":{"marks":-1}},{"$limit":2}])

#### operator $group along with operators like $sum, $avg, $min, $max performs grouping operations
#### below code is aggregation pipeline that prints average marks across all subjects
#### this query is equivalent to sql query: SELECT subject, average(marks) FROM marks GROUP BY subject
    db.marks.aggregate([{"$group":{"_id":"$subject","average":{"$avg":"$marks"}}}])

#### putting together all operators to find top 2 students by average marks, this involves finding average marks per student, sorting output based on average marks in descending order and limiting output to two documents
    db.marks.aggregate([{"$group":{"_id":"$name","average":{"$avg":"$marks"}}},{"$sort":{"average":-1}},{"$limit":2}])

#### Finding total marks for each student across all subjects
    db.marks.aggregate([{"$group":{"_id":"$name","Total sum":{"$sum":"$marks"}}}])

#### Finding maximum marks scored in each subject
    db.marks.aggregate([{"$group":{"_id":"$subject","Maximum Marks":{"$max":"$marks"}}}])

#### Finding minimum marks scored by each student in ascending order
    db.marks.aggregate([{"$group":{"_id":"$name","Minimum marks":{"$min":"$marks"}}},{"$sort":{"Minimum marks":1}}])

#### Finding top two subjects based on average marks
    db.marks.aggregate([{"$group":{"_id":"$subject","AverageSubMarks":{"$avg":"$marks"}}},{"$sort":{"AverageSubMarks":-1}},{"$limit":2}])

#### Creating a collection named movies
    db.createCollection("movies")

#### Loading sample data into movies collection in training database
    db.movies.insertOne({
        "_id": "1",
        "title": "Guardians of the Galaxy",
        "genre": "Action,Adventure,Sci-Fi",
        "Director": "James Gunn",
        "Actors": "Chris Pratt, Vin Diesel, Bradley Cooper, Zoe Saldana",
        "year": 2014,
        "Runtime (Minutes)": 121,
        "rating": "G",
        "Votes": 757074,
        "Revenue (Millions)": 333.13,
        "Metascore": 76
    })

    db.movies.insertOne({
        "_id": "2",
        "title": "Prometheus",
        "genre": "Adventure,Mystery,Sci-Fi",
        "Director": "R_idley Scott",
        "Actors": "Noomi Rapace, Logan Marshall-Green, Michael Fassbender, Charlize Theron",
        "year": 2012,
        "Runtime (Minutes)": 124,
        "rating": "unrated",
        "Votes": 485820,
        "Revenue (Millions)": 126.46,
        "Metascore": 65
    })

    db.movies.insertOne({
        "_id": "3",
        "title": "Split",
        "genre": "Horror,Thriller",
        "Director": "M. Night Shyamalan",
        "Actors": "James McAvoy, Anya Taylor-Joy, Haley Lu Richardson, Jessica Sula",
        "year": 2016,
        "Runtime (Minutes)": 117,
        "rating": "unrated",
        "Votes": 157606,
        "Revenue (Millions)": 138.12,
        "Metascore": 62
    })

    db.movies.insertOne({
        "_id": "4",
        "title": "Sing",
        "genre": "Animation,Comedy,Family",
        "Director": "Christophe Lourdelet",
        "Actors": "Matthew McConaughey,Reese Witherspoon, Seth MacFarlane, Scarlett Johansson",
        "year": 2016,
        "Runtime (Minutes)": 108,
        "rating": "unrated",
        "Votes": 60545,
        "Revenue (Millions)": 270.32,
        "Metascore": 59
    })

    db.movies.insertOne({
        "_id": "5",
        "title": "Suic_ide Squad",
        "genre": "Action,Adventure,Fantasy",
        "Director": "Dav_id Ayer",
        "Actors": "Will Smith, Jared Leto, Margot Robbie, Viola Davis",
        "year": 2016,
        "Runtime (Minutes)": 123,
        "rating": "G",
        "Votes": 393727,
        "Revenue (Millions)": 325.02,
        "Metascore": 40
    })

    db.movies.insertOne({
        "_id": "6",
        "title": "The Great Wall",
        "genre": "Action,Adventure,Fantasy",
        "Director": "Yimou Zhang",
        "Actors": "Matt Damon, Tian Jing, Willem Dafoe, Andy Lau",
        "year": 2016,
        "Runtime (Minutes)": 103,
        "rating": "unrated",
        "Votes": 56036,
        "Revenue (Millions)": 45.13,
        "Metascore": 42
    })

    db.movies.insertOne({
        "_id": "7",
        "title": "La La Land",
        "genre": "Comedy,Drama,Music",
        "Director": "Damien Chazelle",
        "Actors": "Ryan Gosling, Emma Stone, Rosemarie DeWitt, J.K. Simmons",
        "year": 2016,
        "Runtime (Minutes)": 128,
        "rating": "G",
        "Votes": 258682,
        "Revenue (Millions)": 151.06,
        "Metascore": 93
    })

    db.movies.insertOne({
        "_id": "8",
        "title": "Mindhorn",
        "genre": "Comedy",
        "Director": "Sean Foley",
        "Actors": "Essie Davis, Andrea Riseborough, Julian Barratt,Kenneth Branagh",
        "year": 2016,
        "Runtime (Minutes)": 89,
        "rating": "unrated",
        "Votes": 2490,
        "Revenue (Millions)": 50.9,
        "Metascore": 71
    })

    db.movies.insertOne({
        "_id": "9",
        "title": "The Lost City of Z",
        "genre": "Action,Adventure,Biography",
        "Director": "James Gray",
        "Actors": "Charlie Hunnam, Robert Pattinson, Sienna Miller, Tom Holland",
        "year": 2016,
        "Runtime (Minutes)": 141,
        "rating": "unrated",
        "Votes": 7188,
        "Revenue (Millions)": 8.01,
        "Metascore": 78
    })

    db.movies.insertOne({
        "_id": "10",
        "title": "Passengers",
        "genre": "Adventure,Drama,Romance",
        "Director": "Morten Tyldum",
        "Actors": "Jennifer Lawrence, Chris Pratt, Michael Sheen,Laurence Fishburne",
        "year": 2016,
        "Runtime (Minutes)": 116,
        "rating": "G",
        "Votes": 192177,
        "Revenue (Millions)": 100.01,
        "Metascore": 41
    })

#### mongodb query to find year in which most number of movies were released, $sum operator when used with argument 1 is used for counting
    db.movies.aggregate([{"$group":{"_id":"$year","MostMoviesInaYear":{"$sum":1}}}])
    db.movies.aggregate([{"$group":{"_id":"$year","MostMoviesInaYear":{"$sum":1}}},{"$sort":{"MostMoviesInaYear":-1}},{"$limit":1}])

#### mongodb query to count of movies directed by each director
    db.movies.aggregate([{"$group":{"_id":"$director","moviecount":{"$sum":1}}}])

#### query to find out movies released in 2014 
    db.movies.aggregate([{$match:{"year":2014}}])

#### mongodb query to find movies released before year 2015
    db.movies.find({"year":{$lt:2015}})

#### mongodb query to find movies released after year 2015
    db.movies.find({"year":{$gt:2015}})

#### mongodb query to count of movies released before year 2015
    db.movies.aggregate([{$match:{"year":{$lt:2015}}},{$count:"total Movies before 2015"}])

#### mongodb query to count of movies released after year 2015
    db.movies.aggregate([{$match:{"year":{$gt:2015}}},{$count:"total Movies after 2015"}])

#### mongodb query to find movies rated G
    db.movies.aggregate([{$match:{"rating":"G"}}])

#### mongodb query to count movies rated G
    db.movies.aggregate([{$match:{"rating":"G"}},{$count:"total Movies rated G are"}])

#### query to find out average votes for movies released in 2016
    db.movies.aggregate([{$match:{"year":2016}},{"$group":{"_id":"_id","average":{"$avg":"$Votes"}}}])

#### Creating a collection named electronics in training database, uploaded catalog.json file in electronics collection using Mongodb Compass
    db.createCollection("electronics")

#### query to count total laptops
    db.electronics.aggregate([{$match:{"type":"laptop"}},{$count:"total laptops "}])

#### query to count total smart phones
    db.electronics.aggregate([{$match:{"type":"smart phone"}},{$count:"total smart phones "}])

#### query to find number of smart phones with screen size of 6 inches, don't use "" for matching integer values
    db.electronics.aggregate([{$match:{"screen size":6}}])
    db.electronics.aggregate([{$match:{"type":"smart phone"}},{$match:{"screen size":6}},{$count:"total smart phones with 6 inch display"}])

#### query to find average screen size of smart phones
    db.electronics.aggregate([{$match:{"type":"smart phone"}},{"$group":{"_id":"_id","average screen size of smart phones ":{"$avg":"$screen size"}}}])

## Accessing MongoDB using Python

#### this segment is done on AccessingMongoDBfromPython.ipynb file, file is on my github repository 

## MongoDB on linux

#### need 'mongoimport' and 'mongoexport' tools to move data in and out of mongodb database
#### installing these tools by following commands
    wget https://fastdl.mongodb.org/tools/db/mongodb-database-tools-ubuntu1804-x86_64-100.3.1.tgz
    tar -xf mongodb-database-tools-ubuntu1804-x86_64-100.3.1.tgz
    export PATH=$PATH:/home/project/mongodb-database-tools-ubuntu1804-x86_64-100.3.1/bin
    echo "done"

#### Verify by following command
    mongoimport --version

#### starting mongodb on linux
    start_mongo

#### Importing data in diamonds.json into a collection named diamonds and a database named training in mongodb using linux CLI 
    mongoimport -u root -p MTgyMTItc2hlaGFi --authenticationDatabase admin --db training --collection diamonds --file diamonds.json

#### Exporting data from diamonds collection inside training database into a file named mongodb_exported_data.json
    mongoexport -u root -p MTgyMTItc2hlaGFi --authenticationDatabase admin --db training --collection diamonds --out mongodb_exported_data.json

#### Exporting fields _id,clarity,cut,price from training database, diamonds collection into a file named mongodb_exported_data.csv
    mongoexport -u root -p MTgyMTItc2hlaGFi --authenticationDatabase admin --db training --collection diamonds --out \
    mongodb_exported_data.csv --type=csv --fields _id,clarity,cut,price

#### Importing movies.json into mongodb server into a database named entertainment and a collection named movies
    mongoimport -u root -p MTgyMTItc2hlaGFi --authenticationDatabase admin --db entertainment --collection movies --file movies.json

#### Exporting fields _id, "title", "year", "rating" and "director" from 'movies' collection into a file named partial_data.csv
    mongoexport -u root -p MTgyMTItc2hlaGFi --authenticationDatabase admin --db entertainment --collection movies --out partial_data.csv --type=csv\
    --fields _id,title,year,rating,Director