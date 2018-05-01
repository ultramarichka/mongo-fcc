## 1. INSTALL Mongod

Hey hey from learnyoumongo. First, let's get MongoDB installed.
You can download MongoDB from https://www.mongodb.org/downloads.

We will also need to add it to your $PATH.

### HINTS

To verify that `mongod` is installed, you can try running `mongod --version`.

If you are on Windows, you will need to use `mongod.exe` instead.

It should print out something similar to:

    db version v2.6.8
    2015-05-06T09:44:39.362-0500 git version: nogitversion

<blank>

    mongod --bind_ip=$IP --dbpath=data --nojournal --rest "$@"


## 2. CONNECT

Start mongod on port 27017 with data as the dbpath

-------------------------------------------------------------------------------

### HINTS

You may have to create the data directory.

    mkdir data

To start mongo on port 27017, run `mongod --port 27017 --dbpath=./data`.

Then, in another terminal, run `npm install mongodb`.

Then, run `learnyoumongo verify`.

If this lesson is passed, be sure to leave mongod running as it will
be used for the remainder of the exercise.

## 3. FIND (document in a collection)

Here we will learn how to search for documents.

In this exercise the database name is `learnyoumongo`.
So, the url would be something like: `mongodb://localhost:27017/learnyoumongo`

Use the `parrots` collection to find all documents where age
is greater than the first argument passed to your script.

Using `console.log`, print the documents to stdout.

-------------------------------------------------------------------------------

### HINTS

To connect to the database, one can use something like this:

    var mongo = require('mongodb').MongoClient
    mongo.connect(url, function(err, db) {
      // db gives access to the database
    })

To get a collection, one can use db.collection('<collection name>').

To find a document or documents, one needs to call `find()` on the collection.

Find is a little bit different than what we are used to seeing.

To access the arguments you can use the `process.argv` array of strings (the first argument is stored at the third position process.argv[2]). To convert to an integer, you could use `parseInt()`

Here is an example:

    collection.find({
      name: 'foo'
    }).toArray(function(err, documents) {
    
    })

If your program does not finish executing, you may have forgotten to
close the db. That can be done by calling `db.close()` after you
have finished.

### Resources:

  * http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html#find
  * https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/parseInt

```js
//search for documents in the database where age is greater than in process.argv[2]
var mongo = require('mongodb').MongoClient;
var assert = require('assert');
var url = 'mongodb://localhost:27017/learnyoumongo';

mongo.connect(url, function(err, client){
   if(err){throw err.message;}
   //console.log(err);
  // console.log(db, "db", db.collection, "collection");
   var thebase = client.db('learnyoumongo');
   //console.log('thebase\n',thebase);
   var collection = thebase.collection('parrots');
   
   var queryObj = collection.find({ age : { $gt: parseInt(process.argv[2]) } });
   queryObj.toArray(function(err, documents) {    
      assert.equal(null, err); //if(err!=null){throw err;}
      console.log(documents);
    });
    client.close();
});
```

## 4. FIND PROJECT (doc where 'age' > smth)

Here we will learn how to search for documents but only fetch the fields
we need. Also known as `projection in MongoDB`

Use the parrots collection from the database named `learnyoumongo` to
find all documents where `age` is greater than the first argument
passed to your script.

The difference from the last lesson will be that we only want the
name and age properties

Using console.log, print the documents to stdout.

-------------------------------------------------------------------------------

## HINTS

To find a document or documents, one needs to call find() on the collection.

Find is a little bit different than what we are used to seeing.

Here is an example:

    collection.find({
      name: 'foo'
    }, {
      name: 1
    , age: 1
    , _id: 0
    }).toArray(function(err, documents) {
    
    })

If your program does not finish executing, you may have forgotten to
close the db. That can be done by calling `db.close()` after you
have finished.

## Resource:

  * http://docs.mongodb.org/manual/reference/method/db.collection.find/#explicitly-exclude-the-id-field
  * http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html#find

```js
//search for documents in the database where age is greater than in process.argv[2]
var mongo = require('mongodb').MongoClient;
var assert = require('assert');
var url = 'mongodb://localhost:27017/learnyoumongo';

mongo.connect(url, function(err, client){
   if(err){throw err.message;}
   var thebase = client.db('learnyoumongo');
   var collection = thebase.collection('parrots');
   
   var queryObj = collection.find( {  age : { $gt: parseInt(process.argv[2]) }}, 
                                   {projection: {name: 1, age: 1, _id: 0 }});
   queryObj.toArray(function(err, documents) {    
      assert.equal(null, err); //if(err!=null){throw err;}
      console.log(documents);
    });
    client.close();
});
```


