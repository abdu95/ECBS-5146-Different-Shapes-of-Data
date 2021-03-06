---
title: "NoSQL"
teaching: 100
exercises: 160
questions:
- What is polyglot persistence and how solves the data storge problems of the present days?
- What are the common treats of NoSQL solutions?
- How can a data analyst choose the appropriate data solution tailored to her/his task?


objectives:
- Understanding the polyglot persistence
- Understanding eventual consistency
- Understanding SQL VS NoSQL
- Understanding NoSQL families 
- Introducing Key-Value Stores
- Introducing Appache Zeppelin
- Practice with Redis
- Introducing Time Series DBMS 
- Practice with InfluxDB
- Introducing Document Stores
- Practice with MongoDB
- Introducing Column Stores
- Introducing Search Engines
- Practice with Solr
- Introducing Graph DBMS
- Practice with Neo4J

keypoints:
- To go beyond tabular data, we need other technologies than RDBMS
- NoSQL solutions build for high scalability, simplified consistency, finer control over availability could be a solution to persist different shapes of data
- Hundreds, even thousands of NoSQL solution are available, the best way to orient ourselves in this jungle, is to learn the main families



---

> ## Table of Content
> [Lecture](https://github.com/salacika/DE2DSD/blob/main/nosql/nosql.pptx)
>
> [Redis](#redis)
>
> [InfluxDB](#influx)
>
> [MongoDB](#mongo)
>
> [Solr](#solr)
>
> [Neo4j](#neo4j)
{: .discussion}
  

<br/><br/>
<a name="redis"/>
## Redis



#### Links to help you
[https://redis-py.readthedocs.io/en/stable/index.html](https://redis-py.readthedocs.io/en/stable/index.html)

[https://redis.io/commands#](https://redis.io/commands#)

<br>

#### Exercise interface: [http://ceudsd.net](http://ceudsd.net) (Apache Zeppelin)

Username/Password will be distributed during class. Ask on Slack if you haven't got one. 

<br/><br/>

#### Connect to Redis with Python (Use Zeppelin notebook with Python interpreter)
```
import redis
r = redis.Redis(host='dsd-redis.4phq3b.ng.0001.euw1.cache.amazonaws.com', port=6379)
```
{: .language-python}

#### Set a value with a key
```
r.set('foo', 'bar')
```
{: .language-python}

#### Get value by key
```
r.get('foo')
```
{: .language-python}

#### Overwrite with expiration
```
r.set('foo','bar2',ex=5)
```
{: .language-python}

#### Is it there?
```
r.get('foo')
```
{: .language-python}

#### Set a number
```
r.set("something", 10)
r.get('something')
```
{: .language-python}

#### Increase number value
```
r.incr("something")
r.get('something')
```
{: .language-python}

#### Store multiple key-value
```
r.mset({'one': 1, 'two': 2, 'three': 3})
```
{: .language-python}

#### Display all keys stored in DB
```
r.keys()
```
{: .language-python}

#### Retrieve multiple values by key
```
r.mget('one','three')
```
{: .language-python}

> ## REDIS Exercise 
> USING THE DOCUMENTATION, FIND HOW TO DELETE A VALUE BY KEY AND HOW TO CHECK THE EXISTENCE OF A KEY.
{: .challenge}




<br/><br/><br/>
<a name="influx"/>
## InfluxDB

#### Links to help you
[https://docs.influxdata.com/influxdb/v1.0/query_language/data_exploration/](https://docs.influxdata.com/influxdb/v1.0/query_language/data_exploration/)

[https://docs.influxdata.com/influxdb/v1.0/query_language/math_operators/](https://docs.influxdata.com/influxdb/v1.0/query_language/math_operators/)

[https://docs.influxdata.com/influxdb/v1.0/query_language/functions/](https://docs.influxdata.com/influxdb/v1.0/query_language/functions/)

<br/>

#### Exercise interface: [http://ceudsd.net:8082](http://ceudsd.net:8082)

To connect you need to apply connection Settings: [Screenshot help](https://github.com/salacika/DE2DSD/blob/main/nosql/influx.png?raw=true)

<br/><br/>

#### Schema exploration
Let’s explore the databases in this Influx DBMS:
```
SHOW DATABASES
```
{: .language-sql}

Notice “NOAA_water_database” database in the list.  Similar list you get, if you use the top banner database selector. Please select “NOAA_water_database” in this banner.

Now, let’s see the tables in this DB. In Influx tables are called “measurements”.

```
SHOW MEASUREMENTS
```
{: .language-sql}

The columns here are called fields and tags. From your perspective, there is not much difference between them (in reality tags can be accessed faster). These 2 commands are listing the column names for all measurements:

```
SHOW FIELD KEYS 
```
{: .language-sql}

```
SHOW TAG KEYS 
```
{: .language-sql}

#### Data Exploration

Simple SELECT:
```
SELECT * FROM h2o_feet
```
{: .language-sql}

LIMIT function:
```
SELECT * FROM h2o_feet LIMIT 5
```
{: .language-sql}

ORDER BY:
```
SELECT * FROM h2o_feet ORDER BY time DESC LIMIT 10
```
{: .language-sql}

SELECT specified columns:
```
SELECT location,water_level FROM h2o_feet LIMIT 10
```
{: .language-sql}

Let’s try a where clause:
```
SELECT *  FROM h2o_feet WHERE location = 'santa_monica' LIMIT 10
```
{: .language-sql}

> ## INFLUX Exercise 1 
> HOW MANY “DEGREE” MEASUREMENT POINTS WE HAVE IN H2O_TEMPERATURE?
{: .challenge}

> ## INFLUX Exercise 2 
> LIST THE DISTINCT LEVEL DESCRIPTORS FOR H2O_FEET?
{: .challenge}

MEAN as aggregation function:

```
SELECT MEAN(water_level) FROM h2o_feet GROUP BY location 
```
{: .language-sql}

> ## INFLUX Exercise 3 
> BETWEEN 2015-08-19 AND 2015-08-27 HOW MANY DAILY H2O_FEET MEASUREMENTS WERE DONE IN 'coyote_creek'
{: .challenge}

#### Advanced Data Exploration

Arithmetic SELECT:
```
SELECT (water_level * 2) + 4 from h2o_feet LIMIT 10
```
{: .language-sql}

Some statistical functions:
```
SELECT SPREAD(water_level) FROM h2o_feet
SELECT STDDEV(water_level) FROM h2o_feet
SELECT PERCENTILE(water_level,5) FROM h2o_feet WHERE location = 'coyote_creek'
```
{: .language-sql}

The value 1.148 is larger than 5% of the values in water_level where location equals coyote_creek.


> ## INFLUX Exercise4 
> HOW MUCH IS THE AVERAGE H2O TEMPERATURE BY LOCATION ?
{: .challenge}



<br/><br/><br/>
<a name="mongo"/>
## MongoDB



#### Links to help you

[https://docs.mongodb.com/manual/](https://docs.mongodb.com/manual/)

[https://www.w3schools.com/python/python_mongodb_getstarted.asp](https://www.w3schools.com/python/python_mongodb_getstarted.asp)

[AirBnb dataset](https://docs.atlas.mongodb.com/sample-data/sample-airbnb/)

<br/>

#### Exercise interface: [http://ceudsd.net](http://ceudsd.net) (Apache Zeppelin)

Username/Password will be distributed during class. Ask on Slack if you haven't got one. 
<br/><br/>

<br/>

#### Connect to MongoDB with Python (Use Zeppelin notebook with Python interpreter)

```
import pymongo
import pprint
mongo = pymongo.MongoClient("mongodb://xxx:27017")
```
{: .language-python}

<br/>
#### Select a database and a collection

If database or collection does not exist, will be created with the first data write (eg. insert line)

```
db = mongo["mydatabase"]
customers = db["customers"]
```
{: .language-python}

<br/>
#### List collections stored in the database
```
db.list_collection_names()
```
{: .language-python}

<br/>
#### Insert a document
```
id = customers.insert_one({ "name": "John", "address": "Boston Highway 37" }).inserted_id
```
{: .language-python}

<br/>
#### Find 

Find the customer inserted by id. Pretty print the result.
```
pprint.pprint(customers.find_one({"_id": id}))
```
{: .language-python}

Find multiple customers by "name" field. Inverse sort by "address". Limit to 5. In order to print the result we iterating over the result set and pretty print each resulting JSON.


```
for customer in customers.find({"name": "John"}).sort("address",-1).limit(5):
    pprint.pprint(customer)
```
{: .language-python}

<br/>
#### Count 

```
customers.count_documents({"name": "John"})
```
{: .language-python}

<br/>
#### Distinct

Find the customers called "John" which address starts with "Boston" and print out distinct addresses.

```
for customer in customers.find({"name":"John","address": {"$regex": "^Boston"}}).distinct("address"):
    pprint.pprint(customer)
```
{: .language-python}

<br/>
#### Airbnb Sample database
```
airbnb = db["airbnb"]
pprint.pprint(airbnb.find_one())
```
{: .language-python}

<br/>
#### Advance filtering

Filter by a deeper JSON field. Print only part of JSON.

```
for listing in airbnb.find({ "address.country": "Spain" }).limit(10):
    pprint.pprint(listing['address']['government_area'])
```
{: .language-python}

> ## MONGO Exercise 
> COUNT HOW MANY AIRBNB LISTINGS WE HAVE IN THE SAMPLE DATABASE HAVING "COUNTRY_CODE" "US" OR "ADDRESS.MARKET" STARTWITH "M" (USE MONGODB DOCUMENTATION)
>> ## Solution
>> Just a hint, the solution in SQL is something like:
>> ~~~
>> SELECT COUNT(*) FROM airbnb WHERE country_code='US' OR market LIKE 'M%'
>> ~~~
>> {: .language-sql} 
> {: .solution} 
{: .challenge}




<br/><br/><br/>
<a name="solr"/>
## Solr

#### Links to help you
[https://cwiki.apache.org/confluence/display/solr/The+Standard+Query+Parser](https://cwiki.apache.org/confluence/display/solr/The+Standard+Query+Parser)

[http://yonik.com/solr/query-syntax/](http://yonik.com/solr/query-syntax/)

<br/>

#### Exercise interface: [http://ceudsd.net:8081/solr/#/flightdelays/query](http://ceudsd.net:8081/solr/#/flightdelays/query)

<br/><br/>

#### A simple query

SOLR has different connectors to programming languages. For simple query testing, we don’t need to program because SOLR is offering so called HTTP Rest interface. These are basically url calls from a browser.

The simplest query (the result is limited by default to 10):
```
http://ceudsd.net:8081/solr/flightdelays/select?q=*:* 
```
{: .output}

<br/>
In SQL, this would be something like:
```
SELECT * FROM flightdelays LIMIT 10;
```
{: .language-sql}


<br/>
#### Ranges 

List records from the last 10 years where tail number is N520JB:


```
http://ceudsd.net:8081/solr/flightdelays/select?fl=DISTANCE,ORIG_CITY,DEST_CITY&q=TAIL_NUMBER:N520JB AND DATE:[NOW-10YEARS TO *]&sort=DISTANCE desc&rows=5
```
{: .output}

<br/>
In SQL, this would be something like:
```
SELECT distance,orig_city,dest_city FROM flightdelays 
  WHERE tail_number='N520JB' AND date >= DATE_SUB(NOW(),INTERVAL 10 YEAR) 
  ORDER BY distance DESC 
  LIMIT 5;
```
{: .language-sql}

<br/><br/>
#### String search / Fuzzy search
List records where tail numbers starting with any character, followed by “2”, followed by 2 any character, followed by "jb". Display only tail number in the result set:
```
http://ceudsd.net:8081/solr/flightdelays/select?fl=TAIL_NUMBER&q=TAIL_NUMBER:?2??jb
```
{: .output}

Fuzzy searches is based on the Damerau-Levenshtein Distance or Edit Distance algorithm. Fuzzy searches discover terms that are similar to a specified term without necessarily being an exact match. To perform a fuzzy search, use the tilde ~ symbol at the end of a single-word term

In the next example we list records with destination city close to "columbas" by distance of 2. The distance referred to here is the number of term movements needed to match the specified phrase.
```
http://ceudsd.net:8081/solr/flightdelays/select?fl=DEST_CITY&q=DEST_CITY:columbas~2
```
{: .output}

<br/>
#### Facets

Same as before, but this time return back distinct destination cities as well:
```
http://ceudsd.net:8081/solr/flightdelays/select?q=DEST_CITY:columbas~2&facet.field=DEST_CITY_str&facet=on
```
{: .output}

<br/>
This previous result sounds like a combined result of the following SQLs:
```
SELECT * FROM flightdelays WHERE DEST_CITY LIKE 'columbas%' LIMIT 10;

SELECT dest_city, COUNT(*) FROM flightdelays 
  WHERE DEST_CITY LIKE 'columbas%' 
  GROUP BY dest_city;
```
{: .language-sql}


<br/>
#### Geo spacial search

Return back records within a circle defined by center point of 39.85,-104.66 [lat,lon] and diameter of 2 kilometer. Display only ORIG_CITY and ORIG_LOCATION_p in the result set and facests for ORIG_CITY_str.

```
http://ceudsd.net:8081/solr/flightdelays/select?d=2&facet.field=ORIG_CITY_str&facet=on&fl=ORIG_CITY,ORIG_LOCATION_p,&fq={!geofilt}&pt=39.85,-104.66&q=*:*&sfield=ORIG_LOCATION_p
```
{: .output}

> ## SOLR Exercise 
> HOW MANY FLIGHTS ARRIVED IN SAN FRANCISCO WITH NO DELAY ALTHOUGH THEY DEPARTED AT LEAST 50 MINS BEHIND THE SCHEDULE?
{: .challenge}



<br/><br/><br/>
<a name="neo4j"/>
## Neo4j

#### Links to help you

(https://neo4j.com/developer/cypher-query-language/)

(http://neo4j.com/docs/developer-manual/current/cypher/)

(https://cloudfront-files-1.publicintegrity.org/offshoreleaks/neo4j/guide/index.html)

 <br/>
#### Exercise interface: [http://ceudsd.net:8080 ](http://ceudsd.net:8080)

To connect you need to tick "do not use Bolt" in the Settings: [Screenshot help](https://github.com/salacika/DE2DSD/blob/main/nosql/neo4j.png?raw=true)

Username/Password will be distributed during class. Ask on Slack if you haven't got one. 

<br/><br/>

#### Simple queries

In Neo4J the SELECT is called MATCH. One of the simplest query is selecting 25 Officer nodes :

```
MATCH (n:Entity) 
RETURN n.name LIMIT 25

```
{: .language-cypher}

<br/>
In SQL, this would be something like:
```
SELECT n.name FROM Officer AS n LIMIT 25;
```
{: .language-sql}


We can use WHERE clause to filter our result:
```
MATCH (o:Officer)
WHERE o.countries CONTAINS 'Hungary'
RETURN o
```
{: .language-cypher}

<br/>
In SQL, this would be something like:
```
SELECT o.countries FROM officer AS o WHERE o.countries LIKE '%Hungary%';
```
{: .language-sql}




> ## NEO4J Exercise 1 
> RETURN THE FIRST 10 ADDRESS NODES
{: .challenge}

> ## NEO4J Exercise 2
> HOW MANY PROPERTIES AN ADDRESS NODE HAS? 
{: .challenge}

> ## NEO4J Exercise 3 
> RETURN THE FIRST 10 COUNTRIES OF THE ADDRESS NODE. WHAT IS THE LAST COUNTRY IN THE LIST?
{: .challenge}

> ## NEO4J Exercise4 
> HOW MANY ADDRESS NODES HAS 'Mexico' AND 'Monaco' IN THEIR ADDRESS PROPERTY?
{: .challenge}

<br/>
####  Joins

Find the Officers and the Entities linked to them (double MATCH)

```
MATCH (o:Officer) 
MATCH (o)-[r]-(c:Entity)
RETURN o,r,c
LIMIT 50
```
{: .language-cypher}

<br/>
In SQL, this would be something like:
```
SELECT * 
FROM officer as o  
INNER JOIN Entity as c 
USING (relationship)
```
{: .language-sql}


Find the Officers called "aliyev" and Entities related to them:
```
MATCH (o:Officer) 
WHERE toLower(o.name) CONTAINS "aliyev"
MATCH (o)-[r]-(c:Entity)
RETURN o,r,c
```
{: .language-cypher}

Same, but this time return only "DIRECTOR_OF" relations:
```
MATCH (o:Officer) 
WHERE toLower(o.name) CONTAINS "aliyev"
MATCH (o)-[r:DIRECTOR_OF]-(c:Entity)
RETURN o,r,c
```
{: .language-cypher}




<br/>

####  Count

Which country has the most addresses 
```
MATCH (n:Address) 
RETURN n.countries, count(*)
ORDER BY count(*) DESC
LIMIT 10
```
{: .language-cypher}

> ## NEO4J Exercise 5 
> LIST THE NAME AND NUMBER CONNECTIONS OF THE TOP 10 MOST CONNECTED OFFICERS FROM BULGARIA.WHO IS THE NO1?
{: .challenge}


<br/>
#### Node analytics

Return all node labels
```
MATCH (n)
RETURN DISTINCT labels(n) 
```
{: .language-cypher}

Same as before, but using "WITH" 
```
MATCH (n)
WITH labels(n) AS type
RETURN DISTINCT type
```
{: .language-cypher}


Show the average degree by node type:
```
MATCH (n)
WITH labels(n) AS type, size( (n)--() ) AS degree
RETURN type, round(avg(degree)) AS avg
```
{: .language-cypher}

Calculate the degree and clustering_coefficient of a node:
```
MATCH (a:Officer {name: "Portcullis TrustNet (Samoa) Limited"})--(b)
WITH a, count(DISTINCT b) AS n
MATCH (a)--()-[r]-()--(a)
RETURN n as degree, count(DISTINCT r) AS clustering_coefficient
```
{: .language-cypher}
