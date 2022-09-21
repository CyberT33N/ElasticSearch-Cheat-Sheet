# ElasticSearch-Cheat-Sheet
ElasticSearch Cheat Sheet with the most needed stuff..




# Kibana
- GUI for working with elastic search
- http://127.0.0.1:5601/



<br><br>
__________________________________________
__________________________________________
<br><br>



# Aliases

<br><br>

## List all Aliases
```
curl http://localhost:9200/_aliases
# GET _cat/aliases
```



<br><br>
__________________________________________
__________________________________________
<br><br>





# Indices

<br><br>

## List all Indices
```
# GET _cat/indices
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


## Delete all indices
```
curl -X DELETE 'http://localhost:9200/_all'
```


<br><br>


## Delete specific indexes
- **When you delete an index then alle aliases related will be deleted aswell!**
```
curl -X DELETE 'http://localhost:9200/test_0_c'
```















<br><br>
__________________________________________________________________________________________
__________________________________________________________________________________________

<br><br>

# Search

<br><br>

## show all data
```Bash
http://localhost:9200/test_0_c/_search
```


<br><br>

## show specific data
- https://www.elastic.co/guide/en/elasticsearch/reference/current/search-your-data.html
```Bash
GET /my-index-000001/_search
```





























<br><br>
__________________________________________________________________________________________
__________________________________________________________________________________________

<br><br>

# create
```javascript
 const index = 'test_7677_c'
const id = uuidv4()

let dataset = {
    "relation": { "name": "customer" }
}

const res = await client.create({ index, id, body: dataset })
```


















<br><br>
__________________________________________________________________________________________
__________________________________________________________________________________________

<br><br>

# bulk


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
