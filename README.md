# Cosmocloud-assignment2

This repository contains MongoDB aggregation pipelines for various queries on the `sample_mflix` database. Each pipeline is saved in a separate JSON file for easy reference and execution.

## Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Pipeline Descriptions](#pipeline-descriptions)
  - [1.json](#1json)
  - [2.json](#2json)
  - [3.json](#3json)
  - [4.json](#4json)
  - [5.json](#5json)
- [Usage](#usage)
  - [Running Pipelines with MongoDB Shell](#running-pipelines-with-mongodb-shell)
  - [Running Pipelines with MongoDB Compass](#running-pipelines-with-mongodb-compass)
  - [Running Pipelines with PyMongo](#running-pipelines-with-pymongo)
- [Indexing](#indexing)
- [Performance Considerations](#performance-considerations)

## Introduction

The `sample_mflix` database is a sample database provided by MongoDB that contains information about movies, including their titles, genres, cast, IMDb ratings, and comments. This repository provides aggregation pipelines to perform various queries on the `sample_mflix` database.

## Prerequisites

- MongoDB instance with the `sample_mflix` database.
- MongoDB Shell (`mongosh`) or MongoDB Compass for running the pipelines.
- PyMongo driver if using Python to run the pipelines.

## Pipeline Descriptions

### 1.json

**Query**: Find the title of the movie along with all comments associated with it.

**Output Schema**:
```json
{
  "title": "string",
  "comments": [
    {
      "name": "string",
      "email": "string",
      "text": "string",
      "date": "ISODate"
    }
  ]
}
```

### 2.json

**Query**: Count the number of comments for each movie and include the movie title and the count.

**Output Schema**:
```json
{
  "title": "string",
  "commentCount": "integer"
}
```

### 3.json

**Query**: Find the top 5 movies with the highest average IMDb rating and include the title, IMDb rating, and total number of comments for each movie.

**Output Schema**:
```json
{
  "title": "string",
  "imdbRating": "double",
  "commentCount": "integer"
}
```

### 4.json

**Query**: Get a list of all unique cast members across all movies and count how many movies each cast member appeared in.

**Output Schema**:
```json
{
  "castMember": "string",
  "movieCount": "integer"
}
```

### 5.json

**Query**: Find movies released before 1950 that have an average IMDb rating of 7.0 or higher. Include the title, IMDb rating, release year, genres, and the first 2 comments (if any) for each movie.

**Output Schema**:
```json
{
  "title": "string",
  "releaseYear": "integer",
  "genres": ["string"],
  "imdbRating": "double",
  "comments": [
    {
      "name": "string",
      "text": "string"
    }
  ]
}
```

## Usage

### Running Pipelines with MongoDB Shell

1. Open your terminal or command prompt.
2. Connect to your MongoDB instance using the `mongosh` command:
   ```sh
   mongosh "your_connection_string"
   ```
3. Switch to the `sample_mflix` database:
   ```sh
   use sample_mflix
   ```
4. Load the pipeline from the JSON file and run it:
   ```javascript
   load('path/to/1.json')
   db.movies.aggregate(pipeline)
   ```

### Running Pipelines with MongoDB Compass

1. Open MongoDB Compass and connect to your MongoDB instance.
2. Navigate to the `sample_mflix` database and the `movies` collection.
3. Click on the "Aggregations" tab.
4. Copy the pipeline from the JSON file and paste it into the input field.
5. Click the "Run Aggregation" button to execute the pipeline.

### Running Pipelines with PyMongo

1. Install the PyMongo driver if you haven't already:
   ```sh
   pip install pymongo
   ```
2. Use the following Python script to run the aggregation pipeline:
   ```python
   from pymongo import MongoClient
   import json

   client = MongoClient("your_connection_string")
   db = client.sample_mflix

   with open('path/to/1.json') as f:
       pipeline = json.load(f)

   results = db.movies.aggregate(pipeline)

   for result in results:
       print(result)
   ```

## Indexing

To improve the performance of the aggregation pipelines, ensure that the following indexes are created:

```javascript
db.comments.createIndex({ "movie_id": 1 })
db.movies.createIndex({ "_id": 1 })
db.movies.createIndex({ "year": 1 })
db.movies.createIndex({ "imdb.rating": 1 })
db.movies.createIndex({ "cast": 1 })
```

## Performance Considerations

- **Early Filtering**: Use the `$match` stage early in the pipeline to filter out unnecessary documents.
- **Data Reduction**: Use the `$project` stage to include only the necessary fields early in the pipeline.
- **Efficient Comment Retrieval**: Use a sub-pipeline within the `$lookup` stage to sort and limit comments early.
- **Indexing**: Ensure that relevant fields are indexed to improve the performance of the `$match` and `$lookup` stages.

