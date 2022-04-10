# M001 : MongoDB Basics

# Chapter 1 : What is MongoDB?

## What is the MongoDB Batabase?

- The MongoDB Database
    - A Database : Structured way to store and access data
    - A NoSQL databse : Related tables of data
    - NoSQL documentDB : Data in MongoDB is sotred as documents
    - Stored in collections : Documents are stored in collections of documents

## What is a Document in MongoDB?

- *Document*
- A wat to organize and store data as a set of field-value pairs
    
    ```json
    {
        <field> : <value>,
        <field> : <value>,
        "name" : "Lakshmi",
        "title" : "Team Lead",
        "age" : 26
    }
    ```
    
- *Collection*
 - an organized store of documents in MongoDB usually with common fields between documents. There can be many collections per database and many documents per collection

### Lecture Notes

- *Document* - a way to organize and store data as a set of field-value pairs.
- *Field* - a unique identifier for a datapoint.
- *Value* - data related to a given identifier.

## What is MongoDB Atlas?

- Atlas is
    - Database as a service
    - MongoDB is used at the core of atlas for data storage and retrieval
    - Your database in the cloud for this course and beyond
- Cluster deployment
    - Clusters : A group of servers that stores your data
    - Replica set : A few connected MongoDB instances the store the same data
    - Single cluster in Atlas : Automatically configured as a replica set
- Services
    - Manage cluster creation
    - Run and maintain database deployment
    - Use cloud service provider of your choice
    - Experiment with new tools and features

# Chapter 2 : Importing, Exporting, and Querying Data

## How dose MongoDB store data?

- JSON : JavaScript Standard Object Notation
- JSON format
    - Start and end with curly braces `{}`
    - Separate each key and value with a colon `:`
    - Separate each key:value pair with a comma `,`
    - `“keys”` must be surrounded by quotation marks `“”`
        
        → In MongoDB `“keys”` are called `“fields”`
        
- Pros of JSON
    - Friendly
    - Readable
    - Familiar
- Cons of JSON
    - Text-baesd
    - Space-consuming
    - Limited
- BSON : Binary JSON
    - Bridges the gap between binary representation and JSON format
    - Optimized for
        - Speed
        - Space
        - Flexibility
    - High perfomance
    - General-purpose focus
- JSON vs BSON
    - JSON
        - Encoding : UTF-8 String
        - Data Support : String, Boolean, Number, Array
        - Readability : Human and Machine
    - BSON
        - Encoding : Binary
        - Data Support : String, Boolean, Number(Integer, Long, Float, ...), Array, Date, Raw Binary
        - Readability : Machine Only
- MongoDB stores data in BSON, internally and over the network.

## Importing and Exporting Data

- Stored BSON vs Viewed JSON
    - Export to a local machine : JSON
    - Export to a different system : BSON
    - Backup cloud data locally : BSON
- JSON
    - `mongoimport`
        - `mongoeimport --uri "<Atlas Cluster URI>" --drop=<filename>.json`
    - `mongoexport`
        - `mongoexport --uri "<Atlas Cluster URI>" --collection=<collection name> --out=<filename>.json`
- BSON
    - `mongoresotre`
        - `mongoresotre --uri "<Atlas Cluster URI>" --drop dump`
    - `mongodump`
        - `mongodump --uri "<Atlas Cluster URI>"`
- URI string
    - Uniform Resource Identifier
    - srv - establishes a secure connection
    - `mongodb+srv://user:password@clusterURI.mongodb.net/database`

# Chapter 4 : Advanced CRUD Operations

## Query Operators - Comparison

- MQL operators
    - Update Operators
        
        Enable us to modify date in the database
        
        Example : `$inc`, `$set`, `$unset`
        
    - Query Operators
        
        Provide additional ways to locate data within the database
        
    - $ has multiple uses
        
        Precedes MQL operators
        
        Precedes Aggregation pipeline stages
        
        Allows Access to Field Values
        
- Comparison operators
    
    `$eq` = EQual to
    
    `$ne` = Not Equal to
    
    `$gt` > Greater Then
    
    `$lt` < Less Then
    
    `$gte` ≥ Greater Then or Equal to
    
    `$lte` ≤ Less Then or Equal to
    
    `{ <field>: { <operator>: <value> } }`
    

```python
# Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was not Subscriber:
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$ne": "Subscriber" } }).pretty()

# Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was Customer using a redundant equality operator:
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": { "$eq": "Customer" }}).pretty()

# Find all documents where the tripduration was less than or equal to 70 seconds and the usertype was Customer using the implicit equality operator:
db.trips.find({ "tripduration": { "$lte" : 70 },
                "usertype": "Customer" }).pretty()
```

## Query Operators - Logic

- Logic operators
    
    `$and` Match all of the specified query clauses
    
    `$or` At least one of the query clauses is matched
    
    `$nor` Fail to match both given clauses
    
    `{<operator> : [{statement1},{statement2},...]}`
    
    `$not` Negates the query requirement
    
    `{$not: {statement}}`
    
- Implicit `$and`
    
    `$and` is used as the default operator when an operator is not specified.
    
    `{sector : "Mobile Food Vendor - 881", result: "Warning"}`
    
    Is the same as:
    
    `{"$and": [{sector : "Mobile Food Vendor - 881"}, {result : "Warning"}]}`
    
    Find which student ids are > 25 and < 100 in the `sample_training.grades` collections.
    
    `{"$and": [{"student_id": {"$gt": 25}}, {"student_id": {"$lt": 100}}]}`
    
    Better
    
    `{"student_id": {"$gt": 25, "$lt": 100}}`
    
- Explicit `$and`
    
    When you need to include the same operator more the once in a query
    
    Using the `routes` collection find out how many CR2 and A81 airplanes come through the KZN airport?
    
    `{"$or": [{dst_airport: "KZN"}, {src_airport: "KZN"}]}`
    
    and
    
    `{"$or": [{airplane: "CR2"}, {airplane: "A81"}]}`
    

```python
# Find all documents where airplanes CR2 or A81 left or landed in the KZN airport:
db.routes.find({ "$and": [ { "$or" :[ { "dst_airport": "KZN" },
                                    { "src_airport": "KZN" }
                                  ] },
                          { "$or" :[ { "airplane": "CR2" },
                                     { "airplane": "A81" } ] }
                         ]}).pretty()
```

## Expressive Query Operator

- Expressive `$expr`
    
    `$expr` allows the use of aggregation expressions within the query language
    
    `{ $expr: { <expression> } }`
    
    `$expr` allow us to use variables and conditional statements
    
- `$`
    
    denotes the use of an operator
    
    addresses the field value → like pointer in C
    

```python
# Find all documents where the trip started and ended at the same station:
db.trips.find({ "$expr": { "$eq": [ "$end station id", "$start station id"] }
              }).count()

# Find all documents where the trip lasted longer than 1200 seconds, and started and ended at the same station:
db.trips.find({ "$expr": { "$and": [ { "$gt": [ "$tripduration", 1200 ]},
                         { "$eq": [ "$end station id", "$start station id" ]}
                       ]}}).count()
```

## Array Operators

- `$push`
    
    Allows us to add an element to an array.
    
    Turns a field into an array field if it was previously a different type.
    
- `{<array field> : {"$size": <number>}}`
    
    Returns a cursor with all documents where the specified array field is exactly the given length.
    
- `{<array field> : {"$all": <array>}}`
    
    Returns a cursor with all documents in which the specified array field contains all the given elements regardless of their order in the array.
    
- Querying an array field using
    
    An array returns only exact array matches
    
    A single element will return all documents where the specified array field contains this given element.
    

```python
# Find all documents with exactly 20 amenities which include all the amenities listed in the query array:
db.listingsAndReviews.find({ "amenities": {
                                  "$size": 20,
                                  "$all": [ "Internet", "Wifi",  "Kitchen",
                                           "Heating", "Family/kid friendly",
                                           "Washer", "Dryer", "Essentials",
                                           "Shampoo", "Hangers",
                                           "Hair dryer", "Iron",
                                           "Laptop friendly workspace" ]
                                         }
                            }).pretty()
```

## Array Operators and Projection

- Projection
    
    Only include the specific fields in the cursor result.
    
    `db.<collection>.find({ <query> }, { <projection> })`
    
    1 - include the field
    
    0 - exclude the field
    
    Use only 1s or only 0s
    
    `db.<collection>.find({ <query> }, { <field1>: 1, <field2>: 1 })`
    
    or
    
    `db.<collection>.find({ <query> }, { <field1>: 0, <field2>: 0 })`
    
    exception:
    
    `db.<collection>.find({ <query> }, { <field1>: 1, "_id": 0 })` → `_id` field is included by default
    
- `$elemMatch`
    
    `{ <field>: {"$elemMatch": { <field>: <value> }}}`
    
    Matches documents that contain an array field with at least one element that matches the specified query criteria.
    

```python
# Find all documents with exactly 20 amenities which include all the amenities listed in the query array, and display their price and address:
db.listingsAndReviews.find({ "amenities":
        { "$size": 20, "$all": [ "Internet", "Wifi",  "Kitchen", "Heating",
                                 "Family/kid friendly", "Washer", "Dryer",
                                 "Essentials", "Shampoo", "Hangers",
                                 "Hair dryer", "Iron",
                                 "Laptop friendly workspace" ] } },
                            {"price": 1, "address": 1}).pretty()

# Find all documents that have Wifi as one of the amenities only include price and address in the resulting cursor:
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()

# Find all documents where the student in class 431 received a grade higher than 85 for any type of assignment:
db.grades.find({ "class_id": 431 },
               { "scores": { "$elemMatch": { "score": { "$gt": 85 } } }
             }).pretty()

# Find all documents where the student had an extra credit score:
db.grades.find({ "scores": { "$elemMatch": { "type": "extra credit" } }
               }).pretty()
```

## Array Operators and Sub-Documents

- Querying arrays and sub-documents
    
    MQL uses dot-notation to specify the address of nested elements in a documents
    
    To use dot-notation in arrays specify the position of the element in the array.
    
    `db.collection.find({"field1.ohter field.also a field": "value"})`
    

```python
use sample_training

db.trips.findOne({ "start station location.type": "Point" })

db.companies.find({ "relationships.0.person.last_name": "Zuckerberg" },
                  { "name": 1 }).pretty()

db.companies.find({ "relationships.0.person.first_name": "Mark",
                    "relationships.0.title": { "$regex": "CEO" } },
                  { "name": 1 }).count()

db.companies.find({ "relationships.0.person.first_name": "Mark",
                    "relationships.0.title": {"$regex": "CEO" } },
                  { "name": 1 }).pretty()

db.companies.find({ "relationships":
                      { "$elemMatch": { "is_past": true,
                                        "person.first_name": "Mark" } } },
                  { "name": 1 }).pretty()

db.companies.find({ "relationships":
                      { "$elemMatch": { "is_past": true,
                                        "person.first_name": "Mark" } } },
                  { "name": 1 }).count()
```

# Chapter 5 : Indexing and Aggregation Pipeline

## Aggregation Framework

```python
# Find all documents that have Wifi as one of the amenities. Only include price and address in the resulting cursor.
db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()

# Using the aggregation framework find all documents that have Wifi as one of the amenities``*. Only include* ``price and address in the resulting cursor.
db.listingsAndReviews.aggregate([
                                  { "$match": { "amenities": "Wifi" } },
                                  { "$project": { "price": 1,
                                                  "address": 1,
                                                  "_id": 0 }}]).pretty()

# Find one document in the collection and only include the address field in the resulting cursor.
db.listingsAndReviews.findOne({ },{ "address": 1, "_id": 0 })

# Project only the address field value for each document, then group all documents into one document per address.country value.
db.listingsAndReviews.aggregate([ { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country" }}])

# Project only the address field value for each document, then group all documents into one document per address.country value, and count one for each document in each group.
db.listingsAndReviews.aggregate([
                                  { "$project": { "address": 1, "_id": 0 }},
                                  { "$group": { "_id": "$address.country",
                                                "count": { "$sum": 1 } } }
                                ])

```

## sort() and limit()

- `sort()` : Sort documents
    - 1 : Incresing
    - -1 : Decresing
- `limit(n)` : Get just `n` documents in result
- `cursor.sort().limit()`

## Introduction to Indexes

- Indexes
    - Make queries even more efficient
    - Are one of the most impactful ways to improve query performance
- Single field index
    - `db.<collection>.createIndex({"fields": 1})`
- Not perfect for
    - `db.<collection>.find({"field1": 42}).sort("field2": 1)`
- Compound Index
    - `db.<collection>.createIndex({"field1": 1, "field2": 1})`
- It doesn't really matter whether the index was created in increasing or decreasing order when it is a simple single-field index.

## Introduction to Data Modeling

- What is data modeling?
    
    a way to organize fields in a document to support your application performance and querying capabilities.
    

## Upsert - Update or Insert?

- Everything in MQL that is used to locate a document in a collection can also be used to modify this document.
    
    `db.collection.updateOne({<query to locate>}, {<update>})`
    
- Upsert is a hybrid of update and insert, it should only be used when it is needed.
    
    `db.collection.updateOne({<query>, {<update>}, {"upsert": true})`
    
- If upsert is true
    - Is there a match?
        - YES → Update the matched document
        - NO → Insert a new document

```python
db.iot.updateOne({ "sensor": r.sensor, "date": r.date,
                   "valcount": { "$lt": 48 } },
                         { "$push": { "readings": { "v": r.value, "t": r.time } },
                        "$inc": { "valcount": 1, "total": r.value } },
                 { "upsert": true })
```
