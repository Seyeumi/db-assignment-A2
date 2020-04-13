# Assignment A2 MongoDB

## [Assignment link](<MongoDBExercise.pdf>)

### What is sharding?
- *What is sharding in MongoDB*  
  Sharding is a method for distributing data across multiple machines. MongoDB uses sharding to support deployments with very large data sets and high throughput operations. Database systems with large data sets or high throughput applications can challenge the capacity of a single server. For example, high query rates can exhaust the CPU capacity of the server. Working set sizes larger than the system’s RAM stress the I/O capacity of disk drives.  

- *What are the different components required to implement sharding?*  
    A MongoDB sharded cluster consists of the following components:
    **shard**: Each shard contains a subset of the sharded data. Each shard can be deployed as a replica set.
    **mongos**: The mongos acts as a query router, providing an interface between client applications and the sharded cluster.
    **config servers**: Config servers store metadata and configuration settings for the cluster. As of MongoDB 3.4, config servers must be deployed as a replica set (CSRS).

- *Explain architecture of sharding in mongoDB?*
![img](https://docs.mongodb.com/manual/_images/sharded-cluster-production-architecture.bakedsvg.svg)

### MapReduce with mongoDB

*a)* Provide implementation of map and reduce function  
**Map function**
```javascript
var mapTags = function() {
    if(this.hasOwnProperty("entities")) {
        var hashtags = this.entities.hashtags;
        for(var i = 0; i < hashtags.length; i++) {
        emit(hashtags[i].text.toLowerCase(), 1);
        }
    }
};
```
Because we observed some possible spelling mistakes 'angularJs, angularJS, angularjs', we decided to make everything lower case to see the popularity of everything related to angularJS and similar cases.

**Reduce function**
```javascript
var reduceTags = function(key, values) {
    return values.length;
}
```

*b)* Provide execution command for running MapReduce  
Running this requires the 2 other functions to exist.
This will output everything into a new place called "hash_tags"
```javascript
db.tweets.mapReduce(mapTags, reduceTags, {out: "hash_tags"})
```

*c)* Provide top 10 recorded out of the sorted results  
Using a sort function on our mapReduce function we can sort and limit the amount of output to 10 and will show the top 10 hashtags.
```javascript
db.tweets.mapReduce(mapTags, reduceTags, {out: "hash_tags"}).find().sort({ 'value': -1 }).limit(10)

```
 
