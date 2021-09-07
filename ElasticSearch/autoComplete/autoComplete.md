#  검색어 자동완성

### 1) prefix 쿼리

 : text, keyword 타입

```sql
# text : 띄어쓰기 기준 형태소 단위 (후방에서도 검색) 
GET search_logs_v2/_search
{
  "query": {
    "prefix": {
      "keyword": {
        "value": "전대협"
      }
    }
  }
}
# keyword : 작성중 검색 가능
GET search_logs_v2/_search
{
  "query": {
    "prefix": {
      "keyword.keyword": {
        "value": "전대협"
      }
    }
  }
}
```

 정확도 떨어짐 ( ex : 작성중 검색 ..  )

- - - 
### 2) tokenizer ( ngram , edge ngram ) 설정

- ngram : N-gram: 토큰을 정해진 길이만큼 잘라서 토큰화 하는 filter

- edge-ngram : 토큰의 처음부터 설정한 길이만큼 잘라서 토큰화 하는 n-gram


```sql
# 인덱스 매핑 예시
PUT 인덱스명
{
  "settings": {
    "analysis": {
      "analyzer": {
        "autocomplete": {
          "tokenizer": "autocomplete",
          "filter": [
            "lowercase"
          ]
        },
        "autocomplete_search": {
          "tokenizer": "lowercase"
        }
      },
      "tokenizer": {
        "autocomplete": {
          "type": "edge_ngram",
          "min_gram": 2,
          "max_gram": 20,
          "token_chars": [
            "letter",
            "digit"
          ]
        }
      }
    }
  },
  "mappings": {
    "properties": {
      "word": {
        "type": "text",
        "analyzer": "autocomplete",
        "search_analyzer": "autocomplete_search"
      }
    }
  }
}
```

prefix 보완, 중간, 끝단어 제안 

⇒ 검색어 임의 순서로 나타나야 할때 효율 

[Edge Ngram 토큰 레퍼런스](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/analysis-edgengram-tokenizer.html) 

> 영화 나 노래 제목과 같이 널리 알려진 순서가있는 텍스트를 입력하면서 검색 해야하는 경우 완성 제안 기가 Edge N-gram보다 훨씬 더 효율적인 선택입니다. Edge N-gram은 임의의 순서로 나타날 수있는 단어를 자동 완성하려고 할 때 이점이 있습니다.
- - - 

### 3)  completion을 이용한 자동완성

🔖 [suggest API 정리 ](./suggest.md)

<br>

### 4) 유형별 인덱스 생성
🔖 [인덱스 생성 쿼리 정리](./query.md)

---
### ref
- #### 1
  * [참고 - prefix](https://renuevo.github.io/elastic/autocomplete/elastic-autocomplete-1/)
  * [참고 - 서적](http://preview.hanbit.co.kr/2742/sample_ebook.pdf)
- #### 2
  * [참고 -1 ](https://esbook.kimjmin.net/06-text-analysis/6.6-token-filter/6.6.4-ngram-edge-ngram-shingle) 
  * [참고 - 2 ](https://velog.io/@dahea0512/Elasticsearch-Token-Filter-%EC%A0%95%EB%A6%AC)
  * [참고 - analyzer ](https://renuevo.github.io/elastic/autocomplete/elastic-autocomplete-2/)
  * [참고 - 3 ](https://www.skyer9.pe.kr/wordpress/?p=1101)
  * [참고 - 4 ](https://pythonq.com/so/autocomplete/1443043)