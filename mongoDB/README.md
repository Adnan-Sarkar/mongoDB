# MongoDB

MongoDB is a popular open-source NoSQL database management system that storing data in `JSON` like documents `(BSON- Binary JSON)`.

## Why MongoDB?

- **Schema-less**
  This is the most significant key feature for NOSQL DBMS, MongoDB does not required pre-defined schema design. Every `document` inside a `collection` can have different structure which is gives us more flexibility.<br><br>

- **Scalability**
  MongoDB can scale `horizontally` with multiple servers which makes it suitable for handling huge amount of data and high level of traffic.

- **Query Language**
  MongoDB uses JSON based query language which can supports a wide range of queries, including `fiend queries`, `range queries` and `regular expressions`.

- **Indexing**
  MongoDB supports various types of indexes, which is makes query performance

- **Aggregation Framework**
  It has a powerful `aggregation framework` for performing complex queries.

- **Community**
  MongoDB has a large and active community, rich ecosystem of tools, libraries and frameworks which makes it famous day by day.

## Difference Between SQL & NoSQL

| SQL     | NoSQL       |
| ------- | ----------- |
| Tables  | Collections |
| Rows    | Documents   |
| Columns | Fields      |

## Create/Delete Database

Using `mongosh (mongo + shell)` we can check our current database. So, we need to run those command in mongo shell.

- **show databases**

  ```mongoDB
  show dbs
  ```

  or

  ```mongoDB
  show databases
  ```

- **create database**

  ```mongoDB
  // use <database name>
  use employees
  ```

  > :warning: After create a db, it will not show untill you create at least one collection

- **Create Collection**

  ```mongoDB
  // db.createCollection(<collection name>)
  db.createCollection('demo')
  ```

- **show collections**

  ```mongoDB
  show collections
  ```

- **delete collection**

  ```mongoDB
    // db.<collection name>.drop()
    db.demo.drop()
  ```

- **delete database**
  ```mongoDB
    db.dropDatabase()
  ```
  > :warning: it will dropt current selected database

## Insert Operation in MongoDB

- **insert single document**

  ```mongoDB
   db.<collection name>.insertOne({
    field1: value1,
    field2: value2,
    ...
   })
  ```

- **insert multiple documents**

  ```mongoDB
   db.<collection name>.insertMany([
    {field1: value1, ...},
    {field1: value1, ...},
    {field1: value1, ...},
   ])
  ```

  > :warning: Be careful, when you insert many documents, it should be an array of documents

- **show inserted documents**
  ```mongoDB
   db.<collection name>.find()
  ```

> :warning: If a field name contains special characters or spaces or start with digit using quotes ("") is mandatory.

If we insert multiple documents and somehow got an error for any single document, what happend?

In mongoDB, by default all the documents before the error one will be inserted successfully, but after the error document no one will be insert.

But If we want to insert every document expect the error one? Then we should pass the `2nd` parameter to the `inserMany` method which is `{ ordered: false }`.

```mongoDB
   db.<collection name>.insertMany([
    {...},
    {...},
    {...}
    ],
    { ordered: false })
```

> :warning: Collection names & field names are case-sensitive

## Find Operation in MongoDB

- **find single document**

  ```mongoDB
   db.<collection name>.findOne({ key: value });
  ```

- **find multiple documents**
  ```mongoDB
   db.<collection name>.find({ key: value });
  ```

## Import/Export JSON file in MongoDB

- **without array of objects**

  ```mongoDB
   // without array of objects
   {
    ...
   },
   {
    ...
   }
  ```

  ```sh
   mongoimport jsonfileName.json -d <database name> -c <collection name>
  ```

  Example

  ```sh
   mongoimport studentsInfo.json -d university -c students
  ```

- **with array of objects**
  ```mongoDB
   // with array of objects
   [
      {
        ...
      },
      {
        ...
      }
   ]
  ```
  ```sh
   mongoimport jsonfileName.json -d <database name> -c <collection name> --jsonArray
  ```
  Example
  ```sh
   mongoimport studentsInfo.json -d university -c students --jsonArray
  ```

For export,

```sh
 mongoexport -d <database name> -c <collection name> -o <file directory with name>.json
```

`mongoimport` and `mongoexport` are command-line utilities that are part of the `MongoDB Database Tools`. Limited to imports of 16MB or smaller file size.

## Comparison Operators in MongoDB

All of these comparison operators are used for finding documents.

| Comparison Operators | Description                                                                                      |
| -------------------- | ------------------------------------------------------------------------------------------------ |
| $eq                  | Equal Check (==)                                                                                 |
| $ne                  | Not Equal Check (!=)                                                                             |
| $gt                  | Greater than (>)                                                                                 |
| $gte                 | Greater than or Equal (>=)                                                                       |
| $lt                  | Less than (<)                                                                                    |
| $lte                 | Less than or Equal (<=)                                                                          |
| $in                  | selects the documents where the value of a field equals any value in the specified array         |
| $nin                 | selects the documents where the value of a fields are not equal any value in the specified array |

```mongoDB
  db.<collection name>.find(
    {
      <field name> :
        {
          $operator: value
        }
    }
  )
```

Example,

```mongoDB
  db.employees.find(
    {
      salary :
        {
          $gt: 30000
        }
    }
  )
```

`$in` & `$nin`

```mongoDB
  db.<collection name>.find(
    {
      <field name> :
        {
          $operator: [value1, ...]
        }
    }
  )
```

Example,

```mongoDB
  db.employees.find(
    {
      skills :
        {
          $in: ["Javascript", "MongoDB"]
        }
    }
  )
```

It will find all the documents whose skills include "Javascript", "MongoDB" or both.

## Cursors in MongoDB

In MongoDB, when we use the `find()` method to retrieve specific documents from a collection, then this `find()` method returns us a pointer which is `pointer`. Pointer basically points to the specific documents that we want to find.

Or, we can say that a cursor is apointer, and using this pointer we can access the documents.

Now, the `find()` method return the cursor, now to access the document we need to iterate the cursor. In the `mongosh` (mongodb + shell), if the cursor is not assigned to a var keyword (javascript variable) then the mongosh automatically iterates the cursor up to 20 documents.

MongoDb allows us to iterate the cursor manually.

> :warning: If a cursor inactive for 10 min then MongoDB server will automatically close that cursor.

```mongoDB
  var data = db.<collection name>.find()
```

**Cursor Methods**
| Method name |
| ----------- |
| count() |
| limit() |
| sort() |
| skip() |

- **count()**
  count how many documents are pointed by cursor

  ```mongoDB
  var data = db.<collection name>.find().count()
  ```

- **limit()**
  limit the return documents number. limit(3) means, cursor will return first 3 documents.

  ```mongoDB
  var data = db.<collection name>.find().limit(10)
  ```

- **sort()**
  There is two type of sorting, `ascending order` or `descending order`. In this method we have to pass an object, where key should be a field name and value should be 1 (ascending order) or -1 (descending order). Field name is important because this method sorting the documents based on this field value.

  ```mongoDB
  var data = db.<collection name>.find().sort(
    {
      price: 1 // ascending order
    }
  )
  ```

- **skip()**
  skip the documents from the beginning.
  ```mongoDB
  var data = db.<collection name>.find().skip(5)
  // skip first 5 documents
  ```
