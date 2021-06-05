## Aggregation
* mongo "mongodb+srv://<username>:<password>@<cluster>.mongodb.net/admin"(Connection to db)

Using aggregation, we can do the things we used to previously with MQL and go even beyond it. 

The MQL query to search hotels having 'Wifi' as their amenities would be 

```json
 db.listingsAndReviews.find({ "amenities": "Wifi" },
                           { "price": 1, "address": 1, "_id": 0 }).pretty()
```                           
                           
Using aggregate
 
```json
 db.listingsAndReviews.aggregate([
           { "$match":{"amenities":"Wifi"}},
           { "$project":{ "price":1,"address": 1,"_id": 0 }}
           ]).pretty()
```

We can filter out the results into groups.<br> 
Ex. For database sample_airbnb, and listingsAndReviews collection, we can filter out <br>
each place by its country in the following way. 

```json
 db.listingsAndReviews.aggregate([ {"$project":{"address":1}},
           {"$group":{"_id":"$address.country"}}
 		   ])
```

What room types are present in the sample_airbnb.listingsAndReviews collection?
```json
 db.listingsAndReviews.aggregate([ { "$group": { "_id": "$room_type" } }])

```
### Sorting And Limiting the Search Results. 
> Ascending sort & Limit set to :1
>>db.<collection>.find().sort({"pop":1}).limit(1).pretty()

> Ascending sort & Limit set to :10
>> db.<collection>.find().sort({"pop":1}).limit(1).pretty()

> Descending sort 
>> db.<collection>.find().sort({"pop":-1})

> Sorting by multiple attributes
>> db.zips.find().sort({ "pop": 1, "city": -1 })

* Sort, Limit are examples of cursor methods. These are applied to resulting data from the filtering. 

> Question: Youngest biker. 
```json
db.trips.find({"birth year":{"$ne":""}},{"birth year":1}).sort({"birth year":-1}).limit(1)
```
### Indexing.

Indexes can be created for faster sorting/searching. 

> db.trips.createIndex({ "birth year": 1 }) <br>
> db.trips.createIndex({ "start station id": 1, "birth year": 1 })


### Data Modeling. 

Data modeling - a way to organize fields in a document to support your application performance and querying capabilities.

* DATA IS STORED IN THE WAY THAT IT IS USED. 


### Update & Insert - UPSERT. 

Syntax:-
> db.collection.updateOne({<query to locate>},{update>}) 
> <br>db.collection.updateOne({<query to locate>},{update>},"upsert":true)
> <br>By default upsert is false.
> <br>If a match is found, it updates the document. 
> <br>Else it creates new entry.    
> 
> 
> db.iot.updateOne({ "sensor": r.sensor, "date": r.date,
                   "valcount": { "$lt": 48 } },
                         { "$push": { "readings": { "v": r.value, "t": r.time } },
                        "$inc": { "valcount": 1, "total": r.value } },
                 { "upsert": true })

