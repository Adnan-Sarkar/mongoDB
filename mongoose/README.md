## Table of Contents

1. [Introduction to Mongoose](#introduction-to-mongoose)
2. [Schema](#schema)
3. [Schema Types](#schema-types)
4. [Create Schema](#create-schema)
5. [Create Model](#create-model)
6. [Query methods](#query-methods)
7. [Searching Documents Queries](#searching-documents-queries)
   7.1. [find](#find)
   7.2. [findById](#findbyid)
   7.3. [findOne](#findone)
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

## Schema Types

To define schema, we have to specify every field type using valid schema types. Mongoose provide various types such as

- String

  ```mongoDB
  const schema1 = new Schema({ name: String });
  ```

- Number

  ```mongoDB
  const schema1 = new Schema({ age: Number });
  ```

- Date

  ```mongoDB
    const schema1 = new Schema({ createdAt: Date });
  ```

- Buffer

  ```mongoDB
    const schema1 = new Schema({ binData: Buffer });
  ```

- Boolean

  ```mongoDB
    const schema1 = new Schema({ isActive: Boolean });
  ```

- Mixed

  ```mongoDB
    const schema1 = new Schema({ any: Schema.Types.Mixed });
  ```

- ObjectId

  ```mongoDB
    const schema1 = new Schema({ user: Schema.Types.ObjectId });
  ```

- Array

  ```mongoDB
    const schema1 = new Schema({ numbers: [Number] });
    const schema2 = new Schema({ hobbies: [String] });
  ```

- BigInt

  ```mongoDB
    const schema1 = new Schema({ number: BigInt });
  ```

These schema types are commonly used in mongoDB.

## Create Schema

```mongoDB
  const userSchema = new mongoose.Schema({
    name: String,
    email: String,
    id: Number,
    ...
  })

  // another way

  const userSchema = new mongoose.Schema({
    name: {
      type: String
    },
    email: {
      type: String
    },
    id: {
      type: Number
    },
    ...
  })
```

We can define basic rules on schema so that we can't create new documents without fulfilling these requirements.

```mongoDB
  const userSchema = new mongoose.Schema({
    name: {
      type: String,
      trim: true,
      required: true
    },
    email: {
      type: String,
      required: true
    },
    id: {
      type: Number,
      min: 1,
      max: 100,
      required: true
    },
    ...
  })
```

We can't create document without providing the `required` fields and other rules like `min,max` etc.

We can define validation also.

```mongoDB
  const userSchema = new mongoose.Schema({
    name: {
      type: String,
      trim: true,
      required: true
    },
    email: {
      type: String,
      required: true,
      validate: {
        validator: function (value) {
          return /\S+@\S+\.\S+/.test(value);
        },
        message: 'Invalid email format'
      }
    },
    id: {
      type: Number,
      min: 1,
      max: 100,
      required: true
    },
    ...
  })
```

There are more options we can apply to our schema.

## Create Model

After creating a schema for a document our next target is to create actual document in the database. Mongoose mode is the way to interact with mongoDB collection. So, to create real documents we need a model.

```mongoDB
  const User = model("User", userSchema);
```

Creating a model is very simple, we need to pass the collection name and the schema for that collection.

Now, we can create a new document using this model.

```mongoDB
  User.create({
    name: "...",
    email: "...",
    id: ...,
    ...
  })
```

Using create method we can create a new document of users collection.

## Query methods

Using `ODM` like mongoose is useful because they provide us many useful methods to qurey our collections such as finding documents, updating documents etc.

## Searching Documents Queries

### find

`find()` is the most popular query method to retrieved documents from a collection.

```mongoDB
  User.find();
```

This find method will return all documents from `User` collection. We can filtering the documents based on some conditions using mongoDB operators like `$in`, `$gte`, `$eq` etc.
To filtering documents, we have to pass an argument object and the find method will return those documents that fulfilled our filtering conditions.

```mongoDB
  User.find({
    name: "..."
  });
```

This query returns that documents which are same as filtered name.

### findById

This method will find that document which id is same as argument's id and id should be mongoDb document's `_id`.

```mongoDB
  User.findById(id);
```

`findById` return single document because mongoDB `_id` is unique.

### findOne

This method is same as `find` and just return a single document based on provided condition.

```mongoDB
  User.findOne({
    name: "..."
  });
```
