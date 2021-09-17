## keyword , text 타입별 검색결과 차이 
- - - 
### text
- 형태소 기반으로 분리하기 때문에 단어형태로 검색할 경우 전문 검색이 가능
  * 검색시 쿼리스트링 사용할때 쓰임
### keyword
- exact value 즉, 완전 동일한 데이터에 대해 검색시 사용
  * 완전매칭할때 term에 주로 쓰임 

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
```
- `text` 타입 arc_category_path 로 검색 했을땐 result : 0 이었으나 , 
- `keyword` 타입인 arc_category_code로 검색했을땐 결과가 나옴

----
### REF
- [ref- 1](https://stackoverflow.com/questions/48869795/difference-between-a-field-and-the-field-keyword)
- [ref -2](https://www.elastic.co/kr/blog/strings-are-dead-long-live-strings)
