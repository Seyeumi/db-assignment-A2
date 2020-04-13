# Assignment A2 MongoDB

## [Assignment link](<MongoDBExercise.pdf>)

### What is sharding?
- What is sharding in MongoDB
- What are the different components required to implement sharding?
- Explain architecture of sharding in mongoDB?

### MapReduce with mongoDB

a) Provide implementation of map and reduce function
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
Because we observed some possible spelling mistakes 'angularJs, angularJS, angularjs', we decided to make everything lower case to see the popularity of everything related to angularJS.

**Reduce function**
```javascript
var reduceTags = function(key, values) {
    return values.length;
}
```

b) Provide execution command for running MapReduce
```javascript

//Running this requires the 2 other functions to exist
//This will output everything into a new place called "hash_tags"
db.tweets.mapReduce(mapTags, reduceTags, {out: "hash_tags"})
```

c) Provide top 10 recorded out of the sorted results
Using a sort function on our mapReduce function we can sort and limit the amount of output to 10 and will show the top 10 hashtags.
```javascript
db.tweets.mapReduce(mapTags, reduceTags, {out: "hash_tags"}).find().sort({ 'value': -1 }).limit(10)

```
