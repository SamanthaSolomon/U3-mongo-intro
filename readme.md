[![General Assembly Logo](https://camo.githubusercontent.com/1a91b05b8f4d44b5bbfb83abac2b0996d8e26c92/687474703a2f2f692e696d6775722e636f6d2f6b6538555354712e706e67)](https://generalassemb.ly/education/web-development-immersive)

# MongoDB

![logo](./images/mongodb-logo.jpg)

The applications that we've built thus far have suffered from one major
shortcoming: when the user reloads the page, any data or progress they've made
is lost!

Right now, we can only store information in memory, which is wiped when we
restart our programs. We need some way to fix this.

Enter databases...

## Objectives

By the end of this, developers should be able to:

* Explain the use cases of a database, specifically non-relational databases
* Setup a local MongoDB server
* Define the key terms document and collection in the context of MongoDB
* Perform CRUD on documents in a collection using the Mongo CLI
* Build a simple node CLI to query a MongoDB collection

## Databases

There are many ways to store data on a computer (e.g., writing to a text file,
a binary file) but databases offer a number of advantages:

**Performance**: Once we write data to our database, we can be pretty sure it
won't be lost (unless the server catches on fire).

**Speed**: Databases are generally optimized to be fast at retrieving and
updating information. Literally, databases can be 100,000x faster than reading
from a file. This is especially important at scale.

**Consistency**: Databases can enforce rules regarding consistency of data,
especially when handling simultaneous requests to update information.

**Scalability**: Databases can handle lots of requests per second, and many
databases have ways to scale to massive page loads by replicating / syncing
information across multiple versions of a database.

**Querying**: databases make it easy to search, sort, filter and combine
related data using a **Query Language**.

## Types of Databases

There are a number of types of databases that generally fall into two
categories:

1. Relational
2. Non-relational

In a **relational** database, data is organized by columns and rows, much like
an Excel spreadsheet. If you've ever heard of SQL, it is the most popular
relational database ever.

Today, we are going to explore a **non-relational** databases called [MongoDB](https://www.mongodb.com/)

MongoDB is an open-source **document database** that provides:

* High performance
* High availability
* Automatic scaling

MongoDB is more effective when dealing with a high-volume of data with low
complexity and few associations. Those are the technical reasons why you might
use MongoDB over SQL.

For us, as budding developers learning to build full-stack applications, they
offer some additional advantages: MongoDB is generally pretty easy to use and
learn and simple to understand. As you'll see in a bit, with MongoDB, we're
taking JavaScript objects (in the form of JSON) and saving them to a database.

### Terminology

While this is a bit technical, it's worth clarifying some terminology...

* **Database**: The actual set of data being stored. We may create multiple databases on our computer, often one for each application.
* **Database Management System**: The software that lets a user interact (query) the data in a database. Examples are MongoDB, PostgreSQL, MySQL, etc.
* **Database CLI**: A tool offered by most DBMSs that allows us to interact with and query your database from the command line. For MongoDB, we'll use `mongo`. We'll be mostly working in the CLI today.

## Document Database

### A basic example of a `Person` document:

```json
{
  "name": "Sue",
  "age": 26,
  "status": "Active",
  "groups": ["sass", "express"]
}
```


**:question: What do you see in the data above?**
Note - trailing commas okay in JS but NOT in JSON, needs key value pair, keys must be strings, values can be string, number or bolean

### A Document

**A record in MongoDB is a called document.**

* A data structure composed of pairs or fields (keys) and values
  * Similar to JSON objects [JavaScript Object Notation](https://www.mongodb.com/json-and-bson)
  * Stored as BSON [(binary-encoded JSON)](http://bsonspec.org/ )
* A document can support all data types - numbers, strings, booleans, even arrays and other documents (objects)
* Fields may include other documents and arrays of documents
* A document is analogous to a row in a table

<hr>

**:alarm_clock: Activity**

Let's take a look at the official [Mongo Documentaion](https://docs.mongodb.com/manual/introduction/)

<hr>

### More complicated example of a `Restaurant` document:

```json
{
   "_id" : ObjectId("54c955492b7c8eb21818bd09"),
   "address" : {
      "street" : "2 Avenue",
      "zipcode" : "10075",
      "building" : "1480",
      "coord" : [ -73.9557413, 40.7720266 ]
   },
   "borough" : "Manhattan",
   "cuisine" : "Italian",
   "grades" : [
      {
         "date" : ISODate("2014-10-01T00:00:00Z"),
         "grade" : "A",
         "score" : 11
      },
      {
         "date" : ISODate("2014-01-16T00:00:00Z"),
         "grade" : "B",
         "score" : 17
      }
   ],
   "name" : "Vella",
   "restaurant_id" : "41704620"
}
```

What do you see in the data above?

## Collections 

MongoDB stores documents in collections.

* Collections are analogous to `tables` in relational databases
* Does **NOT** require its documents to have the same schema or structure
* Each document stored in a collection must have a unique `_id` field that acts as a primary key

Great, now that we have a high level understanding of what Mongo is and what
purpose it serves, let's look at how to use it!

## Installation / Starting 

Check to make sure MongoDB is installed by running the following command in
a terminal window:

```sh
mongo --version
```

If you already have it installed you should see output like this...

```sh
$ mongo --version

MongoDB shell version v3.6.6
git version: 6405d65b1d6432e138b44c13085d0c2fe235d6bd
OpenSSL version: OpenSSL 1.0.2n  7 Dec 2017
allocator: tcmalloc
modules: none
build environment:
    distmod: ubuntu1604
    distarch: x86_64
    target_arch: x86_64
```

If you already have Mongo installed, skip to the **Mongo Shell** section.
Otherwise, follow the instructions below.

### Installation Instructions

**Proceed only if you don't have installed! Consult the previous section.**

If you already have Mongo installed, skip to the **Mongo Shell** section.

**Mac OS X:**

    1. Install mongodb with brew

        ```bash
        brew install mongodb
        ```

    2. Create the folder mongo will be using to store your databases

        ```bash
        sudo mkdir -p /data/db
        ```

    3. Change permission so your user account owns this folder you just created

        ```bash
        sudo chown -R $(whoami) /data/db
        ```

    > Type these commands exactly as displayed, you don't need to substitute anything.

> [Linux Instructions on the mongodb website](https://docs.mongodb.org/manual/tutorial/install-mongodb-on-ubuntu/)

## Mongo shell 

### Start Mongo:

In order to work with Mongo we first need to start the mongo service. 

So in a new tab in Terminal and lets confirm if you have the mongo service available. 

```
brew services start mongodb-community
```
or

```
mongod --dbpath /users/$(whoami)/data/db/ 
```
or if not on **Catalina**

```
mongod
```

You should see a bunch of output with the prompt hanging:

```bash
// more lines of info preceed this
2019-04-22T10:08:31.456-0400 I NETWORK  [initandlisten] waiting for connections on port 27017
```

> This is good news, `mongod` just starts up a mongo server locally. **NOTE**:
> you need this running in order to use the mongo cli

### Start The Shell

Back in your original Terminal tab:

```
$ mongo
```

> Feels a little bit like a JavaScript REPL

You should see:

```
MongoDB shell version: 3.x.x
connecting to: test
>
```

> The `>` is a good sign that you've entered the Mongo shell.

### Help

Type `help` to get a list of available commands.

```
> help
```

#### Think-Pair-Share (2min)

Based on what you see in the help menu:

* What jumps out as important?
* What might be useful for debugging?

<details>
<summary>Some things that jump out:</summary>

* `db.help()` : help with database commands
* `show dbs`: show database names
* `show collections`:  show collections in current database
* `use <db_name>`: set current database
* `db.foo.find()`: list objects in collection foo

Also:

* `<tab>` key completion
* `<up-arrow>` and the `<down-arrow>` for history.
</details>

### CLI: Creating a Database

In the Mongo shell, let's create our first database, one which we will be using
to store information about restaurants.

In order to create/connect to a new database, we have to tell Mongo to `use`
a specific database that we want to work with:

```
> use restaurant_db
```
> **Note**: `use` will create the database it received as an argument if not
> already initialized and then connect to it

Verify:

```
> db
restaurant_db
```

> **Note**: the `db` variable is provided by Mongo and will point to the
> database you're currently connected to.

Common gotcha - what happens when we run:

```
$ show dbs
```

> **Note**: we don't see `restaurant_db` listed. It isn't until we add
> a document to our database that our db will show up in `show dbs`!

## CLI: Create a record 

### Insert

Use `insert()` to add documents to a collection.

### Insert A Restaurant

``` json
> db.restaurants.insert({
  "name": "Haikan",
  "address" : {
      "street" : "805 V Street NW",
      "zipcode" : 20001
  },
  "cuisine": "Ramen"
})
```

**Important to note**:

> The `db` is the database we’re connected to. In this case, `restaurant_db`.
> `.restaurants` is then referring to a collection in our `restaurant_db`. We
> use the `.insert()` to add a document to the `restaurants` collection (the
> document inside the parentheses).

> `restaurants` doesn't exist at first, but that's okay. It gets created
> automatically the first time we add a document to it!

### Verify the insert

```bash
> show collections

restaurants
```

`restaurants` was saved as a collection. A collection is really just a group of
documents. If you want to explore all the things you can do with a collection,
type `db.collection_name.help()`, or in this case: `db.restaurants.help()`

Now type:

```js
db.restaurants.find()
```

That should return a document with the following fields:

* `name`
* `address`
* `cuisine`

<hr>

**:question: What is surprising/unexpected?**

<hr>

* Where did `restaurants` come from?
* What is `_id`?

> Note: Documentation on [ObjectId](https://docs.mongodb.org/manual/reference/object-id/)

## Review `insert`

```js
// insert
db.your_collection_name.insert({ data as json })
// find
db.your_collection_name.find()
```

New Record:

* If the document passed to the `insert()` method does not contain the `_id` field the Mongo shell automatically adds the field to the document and sets the field’s value to a generated `ObjectId`.

New collection:

* If you attempt to add documents to a collection that does not exist, MongoDB will create the collection for you.

## Dropping a Database

```bash
> use database_to_be_dropped
> db.dropDatabase()
```

Drops the **current** database. Go ahead and drop your database now.

<hr>

**:alarm_clock:** Activty - 5min 

Add a few more restaurants.

Using the Mongo Shell CLI, add at least 4 new restaurant documents to your
`restaurants` collection.

**ProTip**: I recommend you construct your statements in your editor and copy
/ paste. It will help you now & later.

> Prompt: Did anyone insert multiple at one time?

<hr>

Let's recreate the steps together:

<details>
  <summary>How can we tell which database we are connected to currently?</summary>

  > `db`
</details>

1. Create DB
2. Use the appropriate DB
3. Insert multiple restaurants

``` json
db.restaurants.remove({});
db.restaurants.insert([
  {
    "name": "Haikan",
    "address" : {
      "street" : "805 V Street NW",
      "zipcode" : 20001
    },
    "cuisine": "Ramen"
  },
  {
    "name": "Taqueria Habanero",
    "address": {
      "street": "4710 14th Street NW",
      "zipcode": 20010
    },
    "cuisine": "Mexican"
  },
  {
    "name": "Chicken & Whiskey",
    "address": {
      "street": "1738 14th Street NW",
      "zipcode": 20009
    },
    "cuisine": "Peruvian"
  },
  {
    "name": "The Coupe",
    "address": {
      "street": "3415 11th Street NW",
      "zipcode": 20010
    },
    "cuisine": "American"
  },
  {
    "name": "Da Hong Pao",
    "address": {
      "street": "1409 14th Street NW",
      "zipcode": 20005
    }
  }
])

> db.restaurants.count()
```

> Note that there's no `cuisine` key in the last record. Does that matter?

## Primary Keys

* A record’s unique immutable identifier generated upon creation of a new instance.
* In relational databases, the primary key is usually an *id* field, the value of which is typically an *Integer*.
* In MongoDB, the *_id* field is usually a *[BSON](http://docs.mongodb.org/manual/reference/glossary/#term-bson) [ObjectId](http://docs.mongodb.org/manual/reference/glossary/#term-objectid)*.

<hr> 

**:alarm_clock: Activity**

Let's take a moment to look at the Mongo Docs for [Primary Key](https://docs.mongodb.com/manual/reference/glossary/#term-primary-key)

<hr>

## Break 

## CLI: QUERY for Records 

Breaking down the anatomy of a typical query with Mongo:

    db + collection + operation + modification = results

In order to find all restaurants:
```js
> db.restaurants.find()
```

> **Note**: we can format our output to be a little nicer on the eyes by
> chaining the `.pretty()` method to end of our query like so:
> `db.restaurants.find().pretty()`

### Find by Conditions (like SQL's `where`)

We can add conditions to our query to target documents based on matching
key-value pairs:

```js
> db.restaurants.find({name: "Haikan"});
> db.restaurants.find({"address.zipcode": 20001});
```

### CLI: Update a record(s)

http://docs.mongodb.org/manual/core/write-operations-introduction/

```
> db.your_collection.update(
  { criteria },
  {
    $set: { assignments }
  },
  { options }
)
```

> **Note**: the first key value pair is the condition on which to find the
> document you'd like to update, the second is what values you'd like to set,
> and third is any additional options

### You do (15 min):

> Write all these out in your code editor before you run them in the command line.

Take time to think about and execute the appropriate commands so that you:

* Update all restaurants to have a new key-value pair `{state: 'DC'}`
* Add a property of `rating` to at least 2 documents and give it a numerical value between 1-5
* Change the street `address` of a specific restaurant

**Bonus:**

* Add nested sub-documents to each restaurant to that it has many `reviews`
* Store important information about each `review`

> **Note** this what a sample update might look like:

```js
> db.restaurants.update(
  {"name": "Haikan"},
  { $set: { state: "DC" }}
)
```

> **Note**: In order to update multiple documents at a time, make sure to pass
> the `multi` option as true, like so:

```js
db.restaurants.update(
  {},
  {
    $set: { "state": "DC" }
  },
  {multi: true}
)
```

Verify:

```js
> db.restaurants.find().pretty()
```

### CLI: Remove records

```
> db.restaurants.remove({ conditions })
```

### CLI: Add a nested object

> We already did this! (The address 'object' / 'subdocument')

## Closing: Review Mongo's Key Advantages

* Usability
* High Performance
* High Availability
* Automatic Scaling
* No SQL

### Usability

* Documents (i.e. objects) correspond to native data types in many programming languages.
* Schema-less: no need to manage migrations.

### High Performance

* Embedded documents and arrays reduce need for expensive joins (reduces I/O).
* Indexes support faster queries and can include keys from embedded documents and arrays. (More info on MongoDB indexing [here](https://dev.to/akazia_it/introduction-to-mongodb-indexing).)

### High Availability

MongoDB’s replication facility, called replica sets, provide:

* automatic failover.
* data redundancy.

replica set:

> is a group of MongoDB servers that maintain the same data set, providing
> redundancy and increasing data availability.

### Automatic Scaling

* Automatic sharding distributes data across a cluster of machines.
* Replica sets can provide eventually-consistent reads for low-latency high
throughput deployments.

> Interested in learning more about [No SQL?](https://www.mongodb.com/nosql-explained)

## Additional Resources

* [Mongo to SQL Mapping Chart](http://docs.mongodb.org/manual/reference/sql-comparison/)
* [CRUD Intro](http://docs.mongodb.org/manual/core/crud-introduction/)
* [CRUD Commands](http://docs.mongodb.org/manual/reference/crud/)
* [bios Collection](http://docs.mongodb.org/manual/reference/bios-example-collection/)

## [License](LICENSE)

1. All content is licensed under a CC­BY­NC­SA 4.0 license.
1. All software code is licensed under GNU GPLv3. For commercial use or
    alternative licensing, please contact legal@ga.co.
