# FSharp.MongoDb.Driver

This is copy of [FSharp mongo driver prototype](https://github.com/tkellogg/MongoDB.FSharp)
updated to work with latest .Net version.

[Prototype summary](https://www.mongodb.com/blog/post/enhancing-the-f-developer-experience-with-mongodb)

The current implementation mostly convert F# quote expressions to the BsonDocument. 
Then this bson document is used to query collections.

The provided API implementation covering only small part of this standard mongo driver API and there is no intention to fully cover it.
This library is intended to be used in some private projects and provide only API required there.     

## Run mongo in docker

```
// docker run --name local-mongo -v c:/dev/data/mongo:/data/db -d --rm -p 27017:27017 mongo
``` 

## Example

```
    let mongo = MongoClient()
    let db = mongo.GetDatabase("user_logins_x")

    let col =
        db.GetCollection<UserLoginDoc>("logins")

    let keys =
        [| createIndexModel <@ fun x -> x.seqNr @> BsonOrderDesc
           createIndexModel <@ fun x -> x.dateTime @> BsonOrderDesc
           createIndexModel <@ fun x -> x.email @> BsonOrderAsc |]

    createIndexesRange col keys |> ignore

    let res = find col Some(<@ fun x -> x.seqNr > 10 @>) (SortByAsc <@ fun x -> x.dateTime @>)
                |> toList

```

## Linking

```
dotnet add reference ./PATHTOLIBRARY/FSharp.MongoDb.Driver/FSharp.MongoDb.Driver.fsproj
```
