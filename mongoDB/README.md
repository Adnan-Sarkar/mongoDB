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

## Logical Operators in MongoDB

| Logical Operators |
| ----------------- |
| $and              |
| $or               |
| $nor              |
| $not              |

- **$and**
  This operator performs a logical `AND` operation on an array of conditions, and selects the documents that satisfy all the conditions.

  ```mongoDB
    {
      $and: [
        {
          // condition
        },
        {
          // condition
        },
        ...
      ]
    }
  ```

  for example, we have to find all students with cgpa above 3.5 in the CSE department.

  ```mongoDB
    db.students.find({
      $and: [
        {
          cgpa: { $gt: 3.5 }
        },
        {
          department: "CSE"
        }
      ]
    })
  ```

  Using $and operator is explicit, we can also use implicit `and` without using the operator.

  ```mongoDB
     db.students.find({
      cgpa: { $gt: 3.5 },
      department: "CSE"
     })
  ```

  This will treat as a implicit `AND` operation.

- **$or**
  This operator is completely opposite of $and operator, where $and operator must satisfy all conditions but $or operator does not need to satisfy all conditions if one condition is true. Syntax is same as $and operator.

  ```mongoDB
    {
      $or: [
        {
          // condition
        },
        {
          // condition
        },
        ...
      ]
    }
  ```

- **$nor**
  This operator performs the logical `NOR` operation. It's opposite of $and operator because $and operator needs to satisfy all condition whereas $nor operator retrieve only those documents that do not match all the given conditions in the array.

  ```mongoDB
    {
      $nor: [
        {
          // condition
        },
        {
          // condition
        },
        ...
      ]
    }
  ```

- **$not**
  $nor operator is used for an array of conditions whereas it gives us documents that do not match all the given array of conditions, But the `$not`operator does the same task for only one condition. `$not` operator finds all the documents which do not match for the give condition.

  ```mongoDB
    db.<collection name>.find({
      price: { $not: 200 }
    })
  ```

## Elements Operators in MongoDB

| Elements Operators |
| ------------------ |
| $exists            |
| $type              |
| $size              |

- **$exists**
  The `$exists` operator matches documents that contain or do not contain a specified field, also including documents that field value is `null`.

  ```mongoDB
    {
      field: {
        $exists: <boolean>
      }
    }
  ```

- **$type**
  The `$type` operator selects documents where the value of the field matches with the specified `BSON data type`.

  ```mongoDB
    {
      field: {
        $type: <BSON data type>
      }
    }
  ```

- **$size**
  The `$size` operator for matching the size of an array of a field. So, it returned the documents that the provided size matches the field's array size.

  ```mongoDB
    {
      field: {
        $size: <array length>
      }
    }
  ```

## Projections in MongoDB

Projection is used to include the specific fields in the returned documents. For example, if the documents contain huge fields but you need only a few specific fields then the projection is useful for filter out the fields and it will impact the performance.

```mongoDB
  db.<collection name>.find()
  // return document like:
  // {
  //  name: "...",
  //  age: ...,
  //  address: "...",
  //  ...
  // }
```

This returned document has huge amount of fields, but you need only `name`, `age`, then we can use projection.

```mongoDB
  db.<collection name>
  .find()
  .projection({ name: 1, age: 1 })

  // return document like:
  // {
  //  name: "...",
  //  age: ...
  // }
```

Syntax is fixed, So you have to specify the field name with value `1`.

## Specificy Array & Object Field in MongoDB

Until now, we used direct field name with operators, but what happend if the field contain object or array of object?

```mongoDB
{
  name: "...",
  age: "...",
  address: {
    street: "..."
    city: "..."
  }
  education: [
    {
      degreeName: "...",
      ...
    }
  ]
}
```

In this document how to select the field name `city`? or `degreeName`?
To select them using dot notation like `adress.city` or `education.degreeName`.

```mongoDB
  db.<collection name>
  .find({
    "adress.city": "..."
  })

  // or

  db.<collection name>
  .find({
    "education.degreeName": "..."
  })
```

It will return documents that satisfy the condition.

## $all vs $elemMatch

- **$all**
  The `$all` operator selects the documents where the value of a field is an array that contain all the specified elements. So, we have to pass the specified elements into an array and it's ensure that all the elements are presents in the document's specific field which is also an array.

  ```mongoDB
  {
    <field name>: {
      $all: [ <elemetn1>, <elemetn2>, ... ]
    }
  }
  ```

- **$elemMatch**
  Suppose, we have a field which is an array of objects and each object's has name and age field also. Now, if we want to match multiple field then we can use `$elemMatch` operator.

  ```mongoDB
  // document
  {
    ...,
    ...,
    education: [
      {
        degreeName: "...",
        year: ...
      }
    ]
  }
  ```

  using `$elemMatch`

  ```mongoDB
    db.<collection name>
    .find({
      education: {
        $elemMatch: {
          degreeName: "...",
          year: ...,
        }
      }
    })
  ```

  It will find the documents that are matches at least one element of array.

  Suppose we have array of objects and want to check every object's name field, then we should use `$all` operator because `[ <elemetn1>, <elemetn2>, ... ]` every elements have to match with the array of object' name.

  So, we use `$all` for same field, but `$elemMatch` for multiple field.

## Update Operations in MongoDB

- updateOne() and updateMany()
- Removing and renaming fields
- Adding, removing items from array
- Updating embedded documents (array & object)

- **updateOne() and updateMany()**

  ```mongoDB
    db.<collection name>
    .updateOne(
      { filtering documents },
      {
        $set: {
          existingFieldName: newValue,
          newFieldName: newValue,
          ...
        }
      }
    )
  ```

  `filtering documents` means we are select the spcecific document that we want to update, other wise which document should update is create confusion.

  `$set` operator is very sensitive, because it will replace the entire filed/object/array. In the example, we pass `newFieldName` that will be create and if we pass a value instead of object then the entire object will replace by the value.

  ```mongoDB
    db.<collection name>
    .updateMany(
      { filtering documents },
      {
        $set: {
          existingFieldName: newValue,
          newFieldName: newValue,
          ...
        }
      }
    )
  ```

  updateMany() do the same job, but update multiple documents.

- **Removing and renaming fields**

  For removing any field,

  ```mongoDB
    db.<collection name>
    .updateOne(
      { filtering documents },
      {
        $unset: {
          filedName: 1
        }
      }
    )
  ```

  The `$unset` method simply remove the specified field.

  For renaming any field,

  ```mongoDB
    db.<collection name>
    .updateOne(
      { filtering documents },
      {
        $rename: {
          oldFieldName: "newFieldName"
        }
      }
    )
  ```

  The `$rename` method simply rename the specified field.

- **Adding, removing items from array**
  For adding a item in array,

  ```mongoDB
    db.<collection name>
    .updateOne(
      { filtering documents },
      {
        $push: {
          arrayFieldName: "newElement"
        }
      }
    )
  ```

  The `$push` method simply add an item to the specified array field, and it can add duplicate items.

  For removing a document from an array,

  ```mongoDB
    db.<collection name>
    .updateOne(
      { filtering documents },
      {
        $pop: {
          arrayFieldName: 1
        }
      }
    )
  ```

  Pass `$pop` a value of -1 to remove the first element of an array and 1 to remove the last element in an array.

  Or using `$pull` operator,

  ```mongoDB
    {
      $pull: {
        fieldName1: value or condition,
        fieldName2: value or condition,
        ...
      }
    }
  ```

  The `$pull` operator removes from an existing array all values that match a specified condition.

  Or using `$pullAll` operator,

  ```mongoDB
    {
      $pullAll: {
        fieldName1: [ value1, value2, ... ],
        ...
      }
    }
  ```

  The `$pullAll` operator removes all the specified values from an existing array.

- **Updating embedded documents (array & object)**

  For updation a field which is inside an array of object (embedded documents), then we can update using the positional operator `$`.

  ```mongoDB
  // document like
    {
      ...,
      ...,
      interests: [
        {
          bookTitle: "...",
          bookPrice: ...,
          authorName: "...",
          ...
        },
        ...
      ]
    }
  ```

  now for update the bookPrice field where the bookTitle is "...",

  ```mongoDB
   db.<collection name>
    .updateOne(
      {
        _id: ...,
        "interests.bookTitle": "..."
      },
      {
        $set: {
          "interests.$.bookPrice": ...
        }
      }
    )
  ```

  This `$` positional operator identifies an element in an array to update without explicitly specifying the position of the element in the array.

  This `$` positional operator acts as a placeholder for the first element that matches the query document.

## Delete Operations in MongoDB

The delete operations are used to remove documents from a collection. There are 2 main delete methods to perform delete operation, `deleteOne` and `deleteMany`.

```mongoDB
  db.<collection name>.deleteOne( { filtering document } )
```

```mongoDB
  db.<collection name>.deleteMany( { age: 20 } )
```

## Aggregation Framework in MongoDB

Aggregation is the process of performing transformations on documents and combining them to produce computed results.

Aggregation consist of multiple `pipeline` stages, each performing a specific operation on the input data.

Benefits of using aggregation,

- Aggregating data: complex calculations and operations are possible.
- Advanced transformations: Data can be combined, reshaped and computed for insights.
- Effecient processing: aggregation handles large datasets efficiently.

```mongoDB
  db.<collection name>.aggregate(
    [
      // stage - 1
      {
        ....
      },

      // stage - 2
      {
        ....
      },

      ...
    ]
  )
```

The array is mainly the pipeline and every stage is performed one way, first `stage 1` is executed and generate calculated documents then `stage 2` performs the operation on these documents.

Few aggregation frameworks operators like `$match`, `$project`, `$group`, `$sort`, `$limit`, `$addFields`, `$merge`, `$out`, `$unwind`, and many more.

- **$match**

  The `$match` stage is similar to the `find()` method. At this stage, we can filter out documents based on the matched conditions.

  ```mongoDB
  db.<collection name>.aggregate(
    [
      // stage - 1
      {
        $match: {
          conditions
        }
      },

      ...
    ]
  )
  ```

  example,

  ```mongoDB
  db.<collection name>.aggregate(
    [
      // stage - 1
      {
        $match: {
          age: {
            $gte: 18
          }
        }
      },

      ...
    ]
  )
  ```

- **$project**

  This `$project` operator is similar to `project()` method, where we can decide which fields will be include in the every documents result.

  ```mongoDB
  db.<collection name>.aggregate(
    [
      // stage - 1
      {
        $project: {
          name: 1,
          age: 1,
          address: 1,
          ...
        }
      }
    ]
  )
  ```

  We can use more operator like `$add`, `$divide`, `$multiply`, `$subtract`, ... arithmetic operation inside $project stage to reshape/transform any data.

  ```mongoDB
  db.<collection name>.aggregate(
    [
      // stage - 1
      {
        $project: {
          name: 1,
          bonus: {
            $multiply: [ "$salary", 5 ]
          },
          ...
        }
      }
    ]
  )
  ```

  `bonus` field is created in the $project stage and this type of operation we can't do in `project()` method.

  The result documents will have only these fields.<br><br>

- **$group**

  The `$group` stage groups the documents by specified fields and performs aggregate operations on grouped data.

  ```mongoDB
  db.<collection name>.aggregate(
  [
    // stage - 1
    {
      $group: {
        _id: "<field name for grouping>", // required
        field1: { // optional
          <accumulator1> : <expression1>,
          ...
        }
      }
    }
  ]
  )
  ```

  `-id` is create the group based on field that we provide, and there is some `accumulator` operators we can use to perform any specific operations on group data.

  | Accumulator Operator |
  | -------------------- |
  | $avg                 |
  | $count               |
  | $max                 |
  | $min                 |
  | $sum                 |
  | $push                |
  | ...                  |
  | ...                  |

  - $avg

    ```mongoDB
    db.<collection name>.aggregate(
    [
      $group: {
        _id: "$gender",
        avg: {
            $avg: "$age"
        }

      }
    ]
    )
    ```

    The `$avg` operator will avg the `age` of every documents of the group. So, if 5 different groups are created then each group has few documents and $avg operator will calculate avg for 5 different groups.

    ```mongoDB
      // output look like,

      // group 1
      {
        "_id" : "Male",
        "avg" : 47.21052631578947
      },

      // group 2
      {
        "_id" : "Female",
        "avg" : 56.4
      },

      ...
    ```

  - $count

    ```mongoDB
    db.<collection name>.aggregate(
    [
      {
        $group: {
          _id: "$gender",
          count: {
              $count: {}
          }
        }
      }
    ]
    )
    ```

    The `$count` operator will count the total documents number for each group. This `$count` operator does not accept any parameters.

  - $max

    ```mongoDB
    db.<collection name>.aggregate(
    [
      {
        $group: {
          _id: "$gender",
          maxAge: {
              $max: "$age"
          }

        }
      }
    ]
    )
    ```

    The `$max` operator returns the maximum value of specified field for each group.

    The syntax is same for `$min`, `$sum`.<br><br>

  - $push

    ```mongoDB
    db.<collection name>.aggregate(
    [
     {
       $group: {
        _id: "$gender",
        emails: {
            $push: "$email"
        }

       }
     }
    ]
    )
    ```

    The `$push` operator push all the specified field into an array for each group.

    There are more `accumulator` operators but these are commonly used.<br><br>
