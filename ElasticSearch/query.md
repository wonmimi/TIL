###  빈값 찾기 

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

### 데이터 삭제 
인덱스에서 특정 데이터 또는 전체 (query 데이터)를 삭제
```sql
  POST 인덱스명/_delete_by_query
  {
    # 삭제 하려는 데이터 쿼리
    "query": {
      "match_all": {
        
      }
    }
  }
```