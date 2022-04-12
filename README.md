# ElasticSearch-Cheat-Sheet
ElasticSearch Cheat Sheet with the most needed stuff..









# Indexes

<br><br>

## List all Indexes
```
curl http://localhost:9200/_aliases
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
