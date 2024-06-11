# learnSolr
Sure! Apache Solr is an open-source search platform built on Apache Lucene. It is designed for scalability and flexibility, making it a popular choice for building search applications. Let's break down the key concepts and components of Solr in a beginner-friendly manner:

### 1. What is Solr?
Solr is a powerful search engine that allows you to index and search large volumes of text data quickly and efficiently. It is often used in applications where search functionality is critical, such as e-commerce websites, document management systems, and data analytics platforms.

### 2. Core Concepts

#### a. Indexing
Indexing is the process of adding data to Solr so that it can be searched. When you index data, Solr creates an internal representation of the data that allows for fast search and retrieval.

#### b. Documents
In Solr, the basic unit of information is a document. A document is a collection of fields, where each field has a name and a value. For example, a document representing a book might have fields like `title`, `author`, `publication_date`, and `description`.

#### c. Fields
Fields are the individual pieces of data within a document. Each field has a type (e.g., text, integer, date) that determines how Solr processes and indexes the data.

#### d. Schema
The schema defines the structure of the documents and fields in your Solr index. It specifies the field types, how fields should be indexed, and how they should be stored.

#### e. Querying
Querying is the process of searching the indexed data. Solr provides a powerful query language that allows you to search for documents based on various criteria.

### 3. Key Features

#### a. Full-Text Search
Solr supports full-text search, which means you can search for documents based on the content of their text fields. It uses techniques like tokenization, stemming, and stop words to improve search accuracy.

#### b. Faceting
Faceting is a feature that allows you to categorize search results based on specific fields. For example, you can group search results by author, publication date, or any other field.

#### c. Highlighting
Highlighting is a feature that shows the parts of the document that match the search query. This helps users quickly identify relevant information in the search results.

#### d. Scalability
Solr is designed to handle large volumes of data and high query loads. It can be distributed across multiple servers to improve performance and reliability.

### 4. Basic Workflow
docker compose
'''
    

'''
version: '3'
services:
  solr:
    image: solr:9-slim
    ports:
      - "8983:8983"
    networks: 
      - search
    environment:
      ZK_HOST: "zoo:2181"
    depends_on: 
      - zoo
    restart: on-failure
    volumes:
      - solr-data:/var/solr

  zoo:
    image: zookeeper:3.9
    networks: 
      - search
    environment:
      ZOO_4LW_COMMANDS_WHITELIST: "mntr,conf,ruok"
    restart: on-failure
    volumes:
      - zoo-data:/data
      - zoo-datalog:/datalog

networks:
  search:
    driver: bridge

volumes:
  solr-data:
    driver: local
  zoo-data:
    driver: local
  zoo-datalog:
    driver: local

''' docker compose up -d
'''

### Upload a Configset
'''
curl -X POST --header "Content-Type:application/octet-stream" --data-binary @a.zip "http://localhost:8983/solr/admin/configs?action=UPLOAD&name=myConfigSet"
'''

Create a core 
''' Json
curl -X POST http://localhost:8983/api/collections -H 'Content-Type: application/json' -d '
  {
    "name": "books",
    "config": "books",
    "numShards": 1
  }
'

'''
#### b. Defining the Schema
Before you can index data, you need to define the schema. This involves specifying the fields and their types in a configuration file called `schema.xml`.
Navigate to the my_core directory, which is usually located in server/solr/my_core/conf. Here, you'll find a file named managed-schema. This is where you define your schema.

Open the managed-schema file in a text editor and define your fields. Here’s an example schema for a book collection:

<schema name="example" version="1.6">
  <fields>
    <field name="id" type="string" indexed="true" stored="true" required="true" />
    <field name="title" type="text_general" indexed="true" stored="true" />
    <field name="author" type="text_general" indexed="true" stored="true" />
    <field name="publication_date" type="date" indexed="true" stored="true" />
    <field name="description" type="text_general" indexed="true" stored="true" />
  </fields>
</schema>
----
curl -X POST -H 'Content-type:application/json' --data-binary '{
  "add-field": {
    "name": "title",
    "type": "text_general",
    "indexed": true,
    "stored": true
  }
}' http://localhost:8983/solr/mycore/schema

#### c. Indexing Data
Once the schema is defined, you can start indexing data. This involves sending documents to Solr using its API. Solr will process the documents and add them to the index.

#### d. Querying Data
After indexing, you can query the data using Solr's query language. You can perform simple keyword searches or complex queries with filters and sorting.

### 5. Example

Here's a simple example of how to index and query data in Solr using its REST API.

#### a. Indexing a Document

```bash
curl -X POST -H 'Content-Type: application/json' 'http://localhost:8983/solr/mycore/update?commit=true' -d '[
  {
    "id": "1",
    "title": "The Great Gatsby"
  }
]'
```

#### b. Querying for Documents

```bash
curl 'http://localhost:8983/solr/your_core/select?q=title:Gatsby&wt=json'
```

### 6. Solr Administration
Solr comes with an admin interface that allows you to manage cores, view indexed data, and monitor the performance of your Solr instance. You can access it by navigating to `http://localhost:8983/solr` in your web browser.

### 7. Advanced Features

#### a. SolrCloud
SolrCloud is a distributed version of Solr that allows you to scale your search application across multiple servers. It provides features like automatic failover, load balancing, and distributed indexing.

#### b. Plugins and Extensions
Solr supports various plugins and extensions that add additional functionality, such as custom analyzers, query parsers, and response writers.

### Conclusion
Solr is a powerful and flexible search platform that can handle a wide range of search and indexing needs. By understanding its core concepts and features, you can build efficient and scalable search applications. As you become more familiar with Solr, you can explore its advanced features and customization options to tailor it to your specific requirements.


### --------
Questions:
### Solr and SolrCloud 
### Architecture
### Zookeeper
### Indexing, Searching
### API's create 
### indexing 
### schema's 
