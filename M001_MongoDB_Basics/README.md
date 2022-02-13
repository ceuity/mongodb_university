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
-