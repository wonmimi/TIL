### 중복데이터 찾기 
- people index 에서 name이 중복(동명이인)인 데이터를 찾을경우,
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

[참고](https://www.python2.net/questions-145910.htm)