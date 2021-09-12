#  타입별 auto_complete 인덱스 테스트

> v1 
### 1. 인덱스 생성

```sql
PUT auto_complete_wm_ngram
{
  "settings": {
    "analysis": {
      "analyzer": {
        "ngram_analyzer": {
          "tokenizer": "ngram_tokenizer",
          "filter": [
            "lowercase"
          ]
        },
        "edge_ngram_analyzer": {
          "tokenizer": "edge_ngram_tokenizer",
          "filter": [
            "lowercase"
          ]
        }
      },
      "tokenizer": {
        "ngram_tokenizer": {
          "type": "ngram",
          "min_gram": "2",
          "max_gram": "10",
					"token_chars": [
            "letter"
          ]
        },
        "edge_ngram_tokenizer": {
          "type": "edge_ngram",
          "min_gram": "2",
          "max_gram": "10",
					"token_chars": [
            "letter"
          ]
        }
      }
    }
  },
  "mappings": {
    "_doc": {
      "properties": {
        "keyword": {
          "type": "text",
          "fields": {
            "ngram": {
              "type": "text",
              "analyzer": "ngram_analyzer"
            },
            "edgeNgram": {
              "type": "text",
              "analyzer": "edge_ngram_analyzer"
            },
            "completion": {
              "type": "completion"
            }
          }
        }
      }
    }
  }
}
```

[ 인덱스 설정을 바꾸고싶다면 ?](./modify_index.md)

### 2. 데이터 입력

```sql
PUT auto_complete_wm_ngram/_doc/9 (doc_id)
{
  "keyword":"세트구성품"
}
```

### 3. 데이터 조회

```sql
# edge-ngram
GET auto_complete_wm_ngram/_search
{
  "query": {
    "match": {
      "keyword.edgeNgram": "세트"
    }
  }
}

# ngram
GET auto_complete_wm_ngram/_search
{
  "query": {
    "match": {
      "keyword.ngram": "세트"
    }
  }
}

# completion 
GET /auto_complete_wm_ngram/_search?pretty
{
  "suggest": {
    "keyword-suggest": {
      "prefix": "세트",
      "completion": {
        "field": "keyword.completion",
        "skip_duplicates": true
      }
    }
  },
  "size": 20
}

#completion - POST
POST /auto_complete_wm_ngram/_search?pretty
{
  "suggest": {
    "keyword-suggest": {
      "prefix": "세트",
      "completion": {
        "field": "keyword.completion",
        "fuzzy": {
          "fuzziness": 0
        },
        "skip_duplicates":true,
        "size" : 500
      }
    }
  }
}
```

### 4. tokenizer 토큰 분석 조회

```sql
#edge_ngram
GET auto_complete_wm_ngram/_analyze
{
  "text": "아침 드라마세트장",
  "analyzer": "edge_ngram_analyzer"
}

**RESPONSE
{
  "tokens" : [
    {
      "token" : "아침",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "드라",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "드라마",
      "start_offset" : 3,
      "end_offset" : 6,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "드라마세",
      "start_offset" : 3,
      "end_offset" : 7,
      "type" : "word",
      "position" : 3
    },
    {
      "token" : "드라마세트",
      "start_offset" : 3,
      "end_offset" : 8,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : "드라마세트장",
      "start_offset" : 3,
      "end_offset" : 9,
      "type" : "word",
      "position" : 5
    }
  ]
}**

#ngram
GET auto_complete_wm_ngram/_analyze
{
  "text": "아침 드라마세트장",
  "analyzer": "ngram_analyzer"
}

**RESPONSE
{
  "tokens" : [
    {
      "token" : "아침",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "드라",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "드라마",
      "start_offset" : 3,
      "end_offset" : 6,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "드라마세",
      "start_offset" : 3,
      "end_offset" : 7,
      "type" : "word",
      "position" : 3
    },
    {
      "token" : "드라마세트",
      "start_offset" : 3,
      "end_offset" : 8,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : "드라마세트장",
      "start_offset" : 3,
      "end_offset" : 9,
      "type" : "word",
      "position" : 5
    },
    {
      "token" : "라마",
      "start_offset" : 4,
      "end_offset" : 6,
      "type" : "word",
      "position" : 6
    },
    {
      "token" : "라마세",
      "start_offset" : 4,
      "end_offset" : 7,
      "type" : "word",
      "position" : 7
    },
    {
      "token" : "라마세트",
      "start_offset" : 4,
      "end_offset" : 8,
      "type" : "word",
      "position" : 8
    },
    {
      "token" : "라마세트장",
      "start_offset" : 4,
      "end_offset" : 9,
      "type" : "word",
      "position" : 9
    },
    {
      "token" : "마세",
      "start_offset" : 5,
      "end_offset" : 7,
      "type" : "word",
      "position" : 10
    },
    {
      "token" : "마세트",
      "start_offset" : 5,
      "end_offset" : 8,
      "type" : "word",
      "position" : 11
    },
    {
      "token" : "마세트장",
      "start_offset" : 5,
      "end_offset" : 9,
      "type" : "word",
      "position" : 12
    },
    {
      "token" : "세트",
      "start_offset" : 6,
      "end_offset" : 8,
      "type" : "word",
      "position" : 13
    },
    {
      "token" : "세트장",
      "start_offset" : 6,
      "end_offset" : 9,
      "type" : "word",
      "position" : 14
    },
    {
      "token" : "트장",
      "start_offset" : 7,
      "end_offset" : 9,
      "type" : "word",
      "position" : 15
    }
  ]
}
```

---
> v2 가중치 필드 추가 버전

기존 v1 

keyword.completion 에 input , weight 으로 넣으면 에러 

```sql
# 에러
PUT auto_complete_wm_ngram_score/_doc/suggest_1?refresh
{
    "keyword.completion" : [
        {
            "input": "빨간구두",
            "weight":3
        }
    ]
}

```

에러 메세지 

```sql
"type": "mapper_parsing_exception",
        "reason": "Could not dynamically add mapping for field [keyword.completion]. Existing mapping for [keyword] must be of type object but found [text]."
```

suggest 는 score 필드 sort 처리 안됨

⇒ sort 가 [안됨](https://github.com/elastic/elasticsearch/issues/11026) 

```sql
# sort되지않은 결과 나옴
POST /auto_complete_wm_ngram_score/_search?pretty
{
  "suggest": {
    "keyword-suggest": {
      "prefix": "빨간",
      "completion": {
        "field": "keyword.completion",
        "fuzzy": {
          "fuzziness": 0
        },
        "skip_duplicates": true,
        "size": 500
      }
    }
  },
  "sort": [
    {
      "score": {
        "order": "desc"
      }
    }
  ]
}
```

v2 인덱스 생성
- tokenizer , completion 혼합
- 가중치 필드 추가 
- tokenizer 토큰분석 숫자 (digit) 추가
```sql

# 기존 keyword. completion 삭제
PUT auto_complete_new
{
  "settings": {
    "analysis": {
      "analyzer": {
        "ngram_analyzer": {
          "tokenizer": "ngram_tokenizer",
          "filter": [
            "lowercase"
          ]
        },
        "edge_ngram_analyzer": {
          "tokenizer": "edge_ngram_tokenizer",
          "filter": [
            "lowercase"
          ]
        }
      },
      "tokenizer": {
        "ngram_tokenizer": {
          "type": "ngram",
          "min_gram": "2",
          "max_gram": "10",
          "token_chars": [
            "letter",
            "digit"
          ]
        },
        "edge_ngram_tokenizer": {
          "type": "edge_ngram",
          "min_gram": "2",
          "max_gram": "10",
          "token_chars": [
            "letter",
            "digit"
          ]
        }
      }
    }
  },
  "mappings": {
    "_doc": {
      "properties": {
        "keyword": {
          "type": "text",
          "fields": {
            "ngram": {
              "type": "text",
              "analyzer": "ngram_analyzer"
            },
            "edgeNgram": {
              "type": "text",
              "analyzer": "edge_ngram_analyzer"
            }
          }
        },
        "keyword_suggest": {
          "type": "completion"
        },
        "weight": {
          "type": "byte"
        }
      }
    }
  }
}
```

데이터 입력

```sql
# tokenizer - keyword , score
PUT auto_complete_wm_mix/_doc/11
{
  "keyword":"세트장 장소",
  "score":"2"
}

# completion - keyword_suggest
PUT auto_complete_wm_mix/_doc/suggest_4?refresh
{
    "keyword_suggest" : [
        {
            "input": "세트구성품",
            "weight":3
        }
    ]
}
```

데이터 조회 

```sql
조회 
#tokenizeer - score desc

# ngram 
GET auto_complete_wm_ngram/_search
{
  "query": {
    "match": {
      "keyword.ngram": "빨간"
    }
  },
  "sort": [
    {
      "score": {
        "order": "desc"
      }
    }
  ]
}

#suggest - weight desc (디폴트)
POST /auto_complete_wm_ngram/_search?pretty
{
  "suggest": {
    "keyword-suggest": {
      "prefix": "빨간",
      "completion": {
        "field": "keyword_suggest",
        "fuzzy": {
          "fuzziness": 0
        },
        "skip_duplicates": true,
        "size": 500
      }
    }
  }
}
```


---

- ngram + completion 조합이 딱임.

    completion (1글자) ,

     ngram(2글자이상) 검색 글자수 이하로 포함된 결과는 배제 해야할듯 "빨간구두" 했을때 볼빨간사춘기 결과 포함

- edge_ngram은

    "빨간"했을경우 볼빨간 사춘기 X 

    "세트" 했을경우 드라마세트장 안나옴

- min_gram : 1 로 하면

    ex) 빨간만 검색해도 "빨"이 들어간 키워드가 다 나오게 됨