# Bulk Update New Fields

## Update Operation

```javascript
var requests = [];
db.deal.find({}).forEach(document => {
    for (var i = 0; i < document.buddies.length; i++) {
        var newField = 'buddies.'+idxs[i]+'.name';
        var write = {
            'updateOne': {
                'filter': { "_id": document._id },
                'update': {
                    '$set' : {}
                }
            }
        }
        write['updateOne']['update']['$set'][newField] = document.firstname + document.lastname;
        requests.push(write);
    }
    if (requests.length === 40000) {
        db.deal.bulkWrite(requests);
        requests = [];
    }
});
if (requests.length > 0) {
    db.deal.bulkWrite(requests);
 
}
```

## Verify

```
db.rundCommand({getLastError : 1})
```