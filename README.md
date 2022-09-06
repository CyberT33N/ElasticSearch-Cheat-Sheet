# ElasticSearch-Cheat-Sheet
ElasticSearch Cheat Sheet with the most needed stuff..









# Indexes

<br><br>

## List all Indexes
```
curl http://localhost:9200/_aliases
```

<br><br>

## List specific Index
```
curl http://localhost:9200/indexNameHere
```



<br><br>



## Create Index
```
curl -X PUT localhost:9200/name
```

<br><br>


## Delete all indexes
```
curl -X DELETE 'http://localhost:9200/_all'
```
















<br><br>
__________________________________________________________________________________________
__________________________________________________________________________________________

<br><br>

# Search
```Bash
http://localhost:9200/test_0_c/_search
```































<br><br>
__________________________________________________________________________________________
__________________________________________________________________________________________

<br><br>

# POST


## POST with Binary File at Elastic Search
```Bash
curl -XPOST -H 'Content-Type: application/json' localhost:9200/test_0_c/_bulk --data-binary @src/test-c-1.json
```

```javascript
    const indexName = `test_${projectId}_c`
    const url = `http://127.0.0.1:9200/${indexName}/_bulk`
    const payload = await fs.readFile(filePath)
    const headers = { 'Content-Type': 'application/x-ndjson' }

    const res = await axios.post(url, payload, {
        headers, validateStatus: status => status < 600
    })
```
