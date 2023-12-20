## Table of Contents

1. [Introduction to Mongoose](#introduction-to-mongoose)
   <br>

# Introduction to Mongoose

Mongoose is an `Object Data Modeling` (ODM) library for MongoDB. Unlike MongoDB, which is a NoSQL database and does not enforce a specific schema for documents, Mongoose allows developers to define a schema for their data. This schema acts as a blueprint, providing a structured representation of the expected document structure.

Mongoose offers numerous built-in methods that facilitate interaction with MongoDB, making database operations more straightforward and organized.

## Schema

Schema is to define a structure of a document into a collection. This is a great feature for noSQL database, because without schema defination mongoDB collection can accepts any type of document. For example,

```mongoDB
// document 1
{
   name: "...",
   age: 50
}

// document 2
{
   title: "...",
   price: 500
}
```

Without schema, These two different documents can be stored in a collection, which is not good for well structured database.
In this case, mongoose work as a wrapper of mongoDB. We can define a schema for specific collection where it can takes only that defined schema type documents.

Benefits of using schema,

- **Data Validation**
  With a schema, we can enforce data validation rules
  <br>

- **Structure and Clarity**
  Defining schema provides a clear structure for document
  <br>

- **Default Values**
  Allow us to set default values for fields in schema
  <br>

- **Middleware and Hooks**
  Allow middlewares and hooks for execute function before/after certain events
  <br>

- **Indexes**
  Allow to create index based on specific fileds to improve performance.
