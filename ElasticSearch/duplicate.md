### 중복데이터 찾기 
```sql
GET /people/_search
{
  "size": 0, 
  "query": {
    "bool": {
      "must": [
      ]
    }
  },
  "aggs": {
    "duplicateNames": {
      "terms": {
        "field": "name.keyword",
        "min_doc_count": 2,
        "size": 10000
      }
    }
  }
}
```

https://www.python2.net/questions-145910.htm