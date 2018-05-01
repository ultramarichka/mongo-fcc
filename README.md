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

To get a collection, one can use `db.collection('<collection name>')`.

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
var assert = require('assert'); //for handling error
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
## 5. INSERT (doc into collection)

Connect to MongoDB on port 27017.
You should connect to the database named learnyoumongo and insert
a document into the `docs` collection.

The document should be a json document with the following properties:

  * `firstName`
  * `lastName`

firstName will be passed as the first argument to the lesson.

lastName will be passed as the second argument to the lesson.

Use console.log to print out the object used to create the document.

Make sure you use JSON.stringify convert it to JSON.

-------------------------------------------------------------------------------

### HINTS

Remember, one can access the arguments passed by using process.argv.

In order to use the mongo package, one must first require it like:

    var MongoClient = require('mongodb').MongoClient

To connect, use the connect() function of MongoClient.

Ex.

    MongoClient.connect(url, function(err, db) {
      if (err) throw err
    
    })

If you get a Connection Refused error, make sure that `mongod` is still
running.

After you have successfully connected, you will need to specify a collection.
That can be done by calling the collection() function on the db returned
in the callback to connect.

Say you wanted to specify a collection named users:

    var collection = db.collection('users')

To insert a document, one would need to call insert() on the collection, like this:

    
    // inserting document
    // { a : 2 }
    collection.insert({
      a: 2
    }, function(err, data) {
      // handle error
    
      // other operations
    })

If your program does not finish executing, you may have forgotten to
close the db. That can be done by calling `db.close()` after you
have finished.

### Resource

  * http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html#insert

```js
//create and and insert a json document into the docs collection
var mongo = require('mongodb').MongoClient;
//var assert = require('assert');
var url = 'mongodb://localhost:27017/learnyoumongo';

mongo.connect(url, function(err, client){
   if(err){throw err.message;}
   var thebase = client.db('learnyoumongo');
   var collection = thebase.collection('docs');
   
   var makeJson = {
      firstName: process.argv[2],
      lastName: process.argv[3]
    };
  
   collection.insert(makeJson, function(err, result) {  
        if(err!=null){throw err;}
        console.log(JSON.stringify(makeJson)); 
    });
    client.close();
});
```

## 6. UPDATE (doc in the collection)

Here we are going to update a document in the users collection.

The database name will be accessible via process.argv[2].

Say we have a user defined like:

    {
      "name": "Tina",
      "age": 30,
      "username": "tinatime"
    }

We want to change Tina's age from 30 to 40.

For the purpose of this lesson, assume that the username property is unique.

-------------------------------------------------------------------------------

## HINTS

To update a document, one would need to call `update()` on the collection.

Ex.

    
    // document
    // { a: 2, b: 3 }
    
    collection.update({
      a: 2
    }, {
      $set: {
        b: 1
      }
    }, callback)
    
    // document was updated
    // { a: 2, b: 1 }

The first argument to update() is the `query`. This query is what filters the documents that we are wanting to update. The second argument is an object of the properties to update. Pay close attention to the `$set` property. If we were to omit $set, the document would be replaced with the object represented by the second argument.

If your program does not finish executing, you may have forgotten to
close the db. That can be done by calling db.close() after you
have finished.

## Resources

  * http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html#update
  * http://docs.mongodb.org/manual/tutorial/modify-documents/
  * http://docs.mongodb.org/manual/reference/operator/update/set/#set

```js
//
var mongo = require('mongodb').MongoClient;
var assert = require('assert');
var url = 'mongodb://localhost:27017/learnyoumongo';

mongo.connect(url, function(err, client){
   if(err){throw err.message;}
   var thebase = client.db(process.argv[2]);
   var collection = thebase.collection('users');
   
   collection.update({name: "Tina", age: 30}, {
      $set: {
        age: 40
      }
    }, function(err){
       assert.equal(null, err);
       client.close();      //to avoid asyncronous closing of database,  while updating
    }); 
});  
```
official solution
```js
var mongo = require('mongodb').MongoClient
    
    var url = 'mongodb://localhost:27017/' + process.argv[2]
    mongo.connect(url, function(err, db) {
      if (err) throw err
      var collection = db.collection('users')
      collection.update({
        username: 'tinatime'
      }, {
        $set: {
          age: 40
        }
      }, function(err) {
        if (err) throw err
        db.close()
      })
    })
```

## 7. REMOVE (doc from the collection)

This lesson involves removing a document with the given _id.

The database name will be accessible via process.argv[2].

The collection name will be passed as the second argument to your script.

The _id will be passed as the third argument to your script.

-------------------------------------------------------------------------------

## HINTS

To remove a document, one would need to call remove() on the collection.

Ex.

    
    collection.remove({
      name: 'foo'
    }, callback)

The first argument to remove() is the query.

If your program does not finish executing, you may have forgotten to
close the db. That can be done by calling `db.close()` after you
have finished.

## Resource

  * http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html#remove

```js
var mongo = require('mongodb').MongoClient;
var assert = require('assert');
var url = 'mongodb://localhost:27017';

mongo.connect(url, function(err, client){
   if(err){throw err.message;}
   var thebase = client.db(process.argv[2]);
   var collection = thebase.collection(process.argv[3]);
   
   collection.remove({_id: process.argv[4]}, function(err){
     assert.equal(null, err);
     client.close(); 
   });
});
```

## 8. COUNT (documents with certain criteria)

Here we will learn how to count the number of documents that
meet certain criteria.

Use the `parrots` collection from the database named `learnyoumongo` to
count all documents where age is greater than the first argument
passed to your script.

Using console.log, print the number to stdout.

-------------------------------------------------------------------------------

## HINTS

To count the number of documents meeting certain criteria,
we must use the `collection.count()` function.

Here is an example:

    collection.count({
      name: 'foo'
    }, function(err, count) {
    
    })

If your program does not finish executing, you may have forgotten to
close the db. That can be done by calling `db.close()` after you
have finished.

## Resource

  * http://mongodb.github.io/node-mongodb-native/2.2/api/Collection.html#count

```js
//search for documents in the database where age is greater than in process.argv[2]
var mongo = require('mongodb').MongoClient;
var assert = require('assert');
var url = 'mongodb://localhost:27017/learnyoumongo';

mongo.connect(url, function(err, client){
   if(err){throw err.message;}
   var thebase = client.db('learnyoumongo');
   var collection = thebase.collection('parrots');
   
   collection.count( {  age : { $gt: parseInt(process.argv[2]) }}, function(err, count){
      assert.equal(null, err); //if(err!=null){throw err;}
      console.log(count);
      client.close();
   }); 
});
```
