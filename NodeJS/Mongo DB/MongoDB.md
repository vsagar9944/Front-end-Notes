# MongoDB 

# Introduction to MongoDB

MongoDB is an open-source document database that provides high performance, high availability, and automatic scaling.

A record in MongoDB is a document, which is a data structure composed of field and value pairs. MongoDB documents are similar to JSON objects. The values of fields may include other documents, arrays, and arrays of documents.

The advantages of using documents are:

- Documents (i.e. objects) correspond to native data types in many programming languages.
- Embedded documents and arrays reduce need for expensive joins.
- Dynamic schema supports fluent polymorphism.

## Key Features

### High Performance

### Rich Query Language[¶](https://docs.mongodb.com/manual/introduction/#rich-query-language)

### High Availability

### Horizontal Scalability



## Databases

```shell
use myDB
```

### Create a Database

If a database does not exist, MongoDB creates the database when you first store data for that database

```shell
db.myNewCollection1.insertOne( { x: 1 } )
#The insertOne() operation creates both the database myNewDB and the collection myNewCollection1 if they do not already exist.
```

## Collections

MongoDB stores documents in collections. Collections are analogous to tables in relational databases.

### Create a Collection

If a collection does not exist, MongoDB creates the collection when you first store data for that collection.

### Explicit Creation

MongoDB provides the [`db.createCollection()`](https://docs.mongodb.com/manual/reference/method/db.createCollection/#db.createCollection) method to explicitly create a collection with various options, such as setting the maximum size or the documentation validation rules. If you are not specifying these options, you do not need to explicitly create the collection since MongoDB creates new collections when you first store data for the collections.



# Views

Starting in version 3.4, MongoDB adds support for creating read-only views from existing collections or other views.

To create or define a view, MongoDB 3.4 introduces:

- the `viewOn` and `pipeline` options to the existing [`create`](https://docs.mongodb.com/manual/reference/command/create/#dbcmd.create) command (and [`db.createCollection`](https://docs.mongodb.com/manual/reference/method/db.createCollection/#db.createCollection) helper):

  ```
  db.runCommand( { create: <view>, viewOn: <source>, pipeline: <pipeline> } )

  ```

  or if specifying a default [collation](https://docs.mongodb.com/manual/release-notes/3.4/#relnotes-collation) for the view:

  ```
  db.runCommand( { create: <view>, viewOn: <source>, pipeline: <pipeline>, collation: <collation> } )

  ```

- a new [`mongo`](https://docs.mongodb.com/manual/reference/program/mongo/#bin.mongo) shell helper [`db.createView()`](https://docs.mongodb.com/manual/reference/method/db.createView/#db.createView):

  ```
  db.createView(<view>, <source>, <pipeline>, <collation> )
  ```

## Behavior

Views exhibit the following behavior:

### Read Only

Views are read-only; write operations on views will error.

The following read operations can support views:

- [`db.collection.find()`](https://docs.mongodb.com/manual/reference/method/db.collection.find/#db.collection.find)
- [`db.collection.findOne()`](https://docs.mongodb.com/manual/reference/method/db.collection.findOne/#db.collection.findOne)
- [`db.collection.aggregate()`](https://docs.mongodb.com/manual/reference/method/db.collection.aggregate/#db.collection.aggregate)
- [`db.collection.count()`](https://docs.mongodb.com/manual/reference/method/db.collection.count/#db.collection.count)
- [`db.collection.distinct()`](https://docs.mongodb.com/manual/reference/method/db.collection.distinct/#db.collection.distinct)

### Index Use and Sort Operations

- Views use the indexes of the underlying collection.
- As the indexes are on the underlying collection, you cannot create, drop or re-build indexes on the view directly nor get a list of indexes on the view.
- You cannot specify a [`$natural`](https://docs.mongodb.com/manual/reference/operator/meta/natural/#metaOp._S_natural) sort on a view.

### Projection Restrictions

[`find()`](https://docs.mongodb.com/manual/reference/method/db.collection.find/#db.collection.find) operations on views do not support the following [projection](https://docs.mongodb.com/manual/reference/operator/projection/) operators:

- [`$`](https://docs.mongodb.com/manual/reference/operator/projection/positional/#proj._S_)
- [`$elemMatch`](https://docs.mongodb.com/manual/reference/operator/projection/elemMatch/#proj._S_elemMatch)
- [`$slice`](https://docs.mongodb.com/manual/reference/operator/projection/slice/#proj._S_slice)
- [`$meta`](https://docs.mongodb.com/manual/reference/operator/projection/meta/#proj._S_meta)

### Immutable Name

You cannot rename [views](https://docs.mongodb.com/manual/core/views/#).