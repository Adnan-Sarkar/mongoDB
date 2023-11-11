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
