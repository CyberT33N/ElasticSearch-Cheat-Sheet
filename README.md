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
```
PUT /my-index-000001
{
  "settings": {
    "index": {
      "number_of_shards": 1,  
      "number_of_replicas": 0 
    }
  }
}
```

<br><br>


## Delete all indices
- Be carefully to run this, because it will also delete the kibana indixes which are needed to deploy kibana
```
curl -X DELETE 'http://localhost:9200/_all'

# Kibana Dev Tools
DELETE _all
```


<br><br>


## Delete specific indexes
- **When you delete an index then alle aliases related will be deleted aswell!**
```
curl -X DELETE 'http://localhost:9200/test_0_c'

# Kibana Dev Tools
DELETE IndexNameHere
```

<br><br>

```javascript
/**
 * 
 * @param {*} projectId 
 * @returns
 */
const deleteIdx = async projectId => {
    const indices = [`a_${projectId}j0`, `a_${projectId}d0`]

    console.log('List of available Indices: ', indices)

    for (const index of indices) {
        try {
            console.log(`We delete now Index: ${index}`)
            await client.indices.delete({ index })
            console.log(`Index ${index} wurde gelöscht.`)
        } catch (e) {
            console.error(`Fehler beim Löschen des Index ${index}: ${e}`)
        }
    }
}
```































<br><br>
__________________________________________
__________________________________________
<br><br>





# Mapping

<br><br>

## List all mappings
```
GET /ais_7677_c/_mapping
```


































<br><br>
__________________________________________________________________________________________
__________________________________________________________________________________________

<br><br>

# Search

<br><br>

## search all data (https://www.elastic.co/guide/en/elasticsearch/reference/current/search-your-data.html)
- Notice there is a limit of max documents
```Bash
# HTTP
http://localhost:9200/test_0_c/_search

# Kibana
GET /my-index-000001/_search
```


<br><br>

## search specific document with query
```Bash
# Kibana
POST /test_1234_c/_search/
{ "query": {"match": {
  "customer.kundeVorname": "test"
}} }
```




## Search specific document by id
- Also works with events
```shell
GET /indexName/_doc/docIdHere
```
























<br><br>
__________________________________________________________________________________________
__________________________________________________________________________________________

<br><br>

# create


## Method #1
```javascript
/**
 * 
 * @param {string} filePath 
 * @param {string} projectId 
 * @param {string} prefix 
 * @returns {Promise}
 */
const bulkImportEs = async (filePath, projectId, prefix) => {
    const index = `a_${projectId}_${prefix}`
    console.log('bulkImportEs() - index: ', index)

    const data = await fs.readFile(filePath)
    
    const res = await client.bulk({
        index,
        body: data
    })

    if (res.errors) {
        throw new ValidationError('bulkImportEs() - res', { res })
    }

    console.log('bulkImportEs() - res: ', res)
}
```

## Method #2
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

# update

<br><br>

## update specific value in document
```javascript
POST /yourIndexHere/_update/docIdHere
{
  "doc": {
    "permissions": [
            {
              "createdAt" : "2011-12-17T11:06:26.240Z"
            }
    ]
  }
}
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
































<br><br>
__________________________________________
__________________________________________
<br><br>


# Sync MongoDB with Es
```
import mongoose from 'mongoose'
import mongoosastic from 'mongoosastic'
import { Client } from '@elastic/elasticsearch';

// Elasticsearch-Verbindung konfigurieren
const esClient = new Client({ node: 'http://ms-platform:30920' });
const indexName = 'my-index-000001';

// MongoDB-Verbindung herstellen
await mongoose.connect('mongodb://root:xxxxxxxxxxxxxxxxxx@127.0.0.1:37227/test?replicaSet=rs0&authSource=admin&readPreference=primary&directConnection=true');

// MongoDB-Schema definieren
const { Schema } = mongoose
const TestSchema = new Schema({
  name: String
})

// ElasticSearch-Plugin hinzufügen
TestSchema.plugin(mongoosastic, {
  esClient: esClient,
  index: indexName
});

// MongoDB-Modell erstellen
const Test = mongoose.model('Test', TestSchema);

// Neues Dokument erstellen
const newDocument = new Test({
  name: 'lisa'
});

// Dokument in MongoDB speichern
await newDocument.save();

newDocument.on('es-indexed', function(err, res){
  if (err) throw err;
  /* Document is indexed */
});

// Das Dokument aktualisieren
const updatedDoc = await Test.findOneAndUpdate({ name: 'lisa' }, { name: 'lisa2' }, { new: true });

console.log('Dokument erfolgreich aktualisiert:', updatedDoc);
```

