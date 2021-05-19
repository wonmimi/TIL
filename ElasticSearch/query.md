- 빈값 찾기 

인덱스에서 image_url이 "" 인데이터 조회
#
ex) 
```sql
GET /people_backup/_search
{
  "query": {
    "bool": {
      "must": [
        {
          "exists": {
            "field": "image_url"
          }
        }
      ],
      "must_not": [
        {
          "wildcard": {
            "image_url": "*"
          }
        }
      ]
    }
  }
}
```