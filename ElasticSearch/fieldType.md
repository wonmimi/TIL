## keyword , text 타입별 검색결과 차이 
- - - 
### keyword
완전매칭할때 term에 주로 쓰임 
### text
검색시 쿼리스트링 사용할때 쓰임
```sql
GET /articles/_search
{
  "size": 100, 
  "query": {
    "bool": {
      "must": [
        {
          "terms": {
            "site": [
              "chosunbiz"
            ]
          }
        },
        {
          "term": {
            "arc_migration": {
              "value": true
            }
          }
        },
        {
          "term": {
            "arc_category_code": {
              "value": "/archive"
            }
          }
        },
        {
          "range": {
            "regdate": {
              "lt": "1998-01-01T00:00:00+0900",
              "gte": "1997-01-01T00:00:00+0900"
            }
          }
        }
      ]
    }
  },
  "sort": [
    {
      "regdate": {
        "order": "desc"
      }
    }
  ]
}
````
text 타입 arc_category_path 로 검색 했을땐 result : 0 이었으나 , 
keyword 타입인 arc_category_code로 검색했을떈 결과가 나옴
