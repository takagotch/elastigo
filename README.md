### elastigo
---
https://github.com/mattbaird/elastigo

```
go get github.com/tools/godep

cd $HOME/gopath/src/github.com/myuser/myproject
go build ./...

godep save github.com/myuser/myproject
git add Godeps

godep save ./...

git clone --recursive git@github.com:mattbaird/elastigo.git
docker run -d -p 9200:9200 -p 9300:9300 dockerfile/elasticsearch

boot2docker stop

docker run -d -p 9200:9200 -p 9300:9300 dockerfile/elasticsearch
go test -v -host localhost -loaddata
go test -v ./...
```

```go
for i in {9200..9300}; do
  VBoxManager modifyvm "boot2docker-vm" --natpf1 "tcp-port$i,tcp,,$i,,$i";
  VBoxManager modifyvm "boot2docker-vm" --natpf1 "udp-port$i,udp,,$i,,$i";
done
```

```go
import "github.com/mattbird/elastigo/api"
import "github.com/mattbaird/elastigo/core"

type Tweet struct {
  User string `json:"user"`
  Message string `json:"message"`
}

api.Domain = "localhost"

response, _ := core.Index("twitter", "tweet", "1", nil, Tweet{"kimchy", "Search is cool"})

tw := Tweet{"kimchy", "Search is cool part 2"}
bytesLine, err := json.Marshal(tw)
response, _ := core.Index("twitter", "tweet", "2", nil, bytesLine)

t := time.Now()
core.IndexBulk("twitter", "tweet", "3", &t, Tweet{"kimchy", "Search is now cooloer"})

searchJson := `{
  "query": {
    "term" : { "user" : "kimchy" }
  }
}`
out, err := core.SearchRequest(true, "twitter", "tweet", searchJson, "")
if len(out.Hits.Hits) == 1 {
  fmt.Println(string(out.Hits.Hits[0].Source))
}


import "github.com/mattbaird/elastigo/api"
import "github.com/mattbaird/elastigo/core"

api.Domain = "localhost"

out, err := Search("github").Size("1").Facet(
  Facet().Fields("actors").Size("500"),
).Query(
  Query().Range(
    Range().Field("created_at").From("2018-12-10T15:00:00-08:00").To("2012-12-10T15:10:00-08:00"),
  ).Search("add"),
).Result()

out, err := Search("github").Type("Issues").Pretty().Query(
  Query().Range(
    Range().Field("created_at").From("2012-12-10T15:00:00-08:00").To("2012-12-10T15:00-08:00  "),
  ).Search("add"),
).Result()

out, err := Search("github").Type("Issues").Size("100").Search("add").Result()


qry := map[string]interface{}{
  "query":map[string]interface{}{
    "term":map[string]string{"user": "kimchy"},
  },
}
core.SearchRequest(true, "github", "Issues", qry, "", 0)


core.SearchUri("github", "Issues", "user:kimchy", "", 0)


out, err := Search("github").Filter(
  Filter().Exists("repository.name"),
).Result()


import "github.com/mttbaird/elasticgo/api"
import "github.com./mattaird/elastigo/core"

api.Domain = "localhost"

indexer := core.NewBulkIndexerErrors(10, 60)
done := make(chan bool)
indexer.Run(done)

go func() {
  for errBuf := range indexer.ErrorChannel {
    fmt.Println(errBuf.Err)
  }
}()
for i := 0; i < 20; i++ {
  indexer.Index("twitter", "user", strconv.Itoa(i), "", nil, `{"name":"bob"}`, false)
}
done <- true
```

