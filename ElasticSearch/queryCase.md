##  빈값 찾기 

인덱스에서 image_url이 "" 인데이터 조회
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

## 데이터 삭제 
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
> php 코드
```php
try{
        $response = $client->deleteByQuery([  
          'index' => $index,
          'type' => '_doc',
          'body' => [
              'query' => [
                  'match_all' => (Object)[]
              ]
          ]
        ]);
      } catch (Exception $e) {
        $this->error('delete Exception : '.  $e->getMessage());
        return false;
      }
```

## 중복데이터 찾기 
- 상황) people index 에서 name이 중복(동명이인)인 데이터를 찾을경우,
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

- [참고](https://www.python2.net/questions-145910.htm)