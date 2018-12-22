# ObjectID as a Shard Key
For Sharded Cluster with Ranged Sharding strategy, using ObjectID as a shard key will result in unbalanced load among cluster and will ended up using only one shard cluster during specific time range, and only some of the chunks will become the hotzone.  
Since all the documents created during this time range will be in the same chunk, until chunk split processor devide the chunk, and then finally chunk has moved to different cluster during balancing round.  
MongoDB server will judge if chunk should be splited after every write operation on that chunk, however balacing chunk requires high load on 'from' and 'to' shard server (INSERT, REMOVE, re-building index, etc), and even the config database will be locked for chunk migration.  
Only solution to handle this is to use hashed sharding instead of ranged sharding


## Chunk Migration Log
Chunk migration logs are in in changelog collection.
```
use config
db.changelog.find().sort({$natural:-1}).pretty()
```

## Chunk Status
```
sh.status()
db.collection.getShardDistribution()
```

## Checking the Chunk Size
The mongo shell does not expose simple command to check the chunk size.
```javascript
var ns = "database.collection"
var key = {_id: 1}
db.getSiblingDB("config").chunks.find({ ns: ns }).forEach(
    function (chunk) {
        var ds = db.getSiblingDB(ns.split(".")[0]).runCommand(
            {
                datasize: chunk.ns,
                keyPattern: key,
                min: chunk.min,
                max: chunk.max,
                from : chunk
 
            });
        // printjson(ds);
        print("Chunk: " + chunk._id + " has a size of " + ds.size + ", and includes " + ds.numObjects + " objects (took " + ds.millis + "ms)")
    }
)
```


