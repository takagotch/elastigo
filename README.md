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






















































```

