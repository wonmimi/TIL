- 빈값 찾기 

people_backup 인덱스에서 image_url이 "" 인데이터 조회
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