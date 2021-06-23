# 2. 타입별 auto_complete 인덱스 테스트

: tokenizer , completion 타입 필드별 인덱스 생성 테스트 
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
          "max_gram": "10"
        },
        "edge_ngram_tokenizer": {
          "type": "edge_ngram",
          "min_gram": "2",
          "max_gram": "10"
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

[ 인덱스 설정을 바꾸고싶다면 ?](../modify_index.md)

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
      "prefix": "빨",
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
      "prefix": "빨",
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
  "text": "세트구성품",
  "analyzer": "edge_ngram_analyzer"
}

**RESPONSE**
{
  "tokens" : [
    {
      "token" : "세트",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "세트구",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "세트구성",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "세트구성품",
      "start_offset" : 0,
      "end_offset" : 5,
      "type" : "word",
      "position" : 3
    }
  ]
}

#ngram
GET auto_complete_wm_ngram/_analyze
{
  "text": "세트구성품",
  "analyzer": "ngram_analyzer"
}

**RESPONSE**
{
  "tokens" : [
    {
      "token" : "세트",
      "start_offset" : 0,
      "end_offset" : 2,
      "type" : "word",
      "position" : 0
    },
    {
      "token" : "세트구",
      "start_offset" : 0,
      "end_offset" : 3,
      "type" : "word",
      "position" : 1
    },
    {
      "token" : "세트구성",
      "start_offset" : 0,
      "end_offset" : 4,
      "type" : "word",
      "position" : 2
    },
    {
      "token" : "세트구성품",
      "start_offset" : 0,
      "end_offset" : 5,
      "type" : "word",
      "position" : 3
    },
    {
      "token" : "트구",
      "start_offset" : 1,
      "end_offset" : 3,
      "type" : "word",
      "position" : 4
    },
    {
      "token" : "트구성",
      "start_offset" : 1,
      "end_offset" : 4,
      "type" : "word",
      "position" : 5
    },
    {
      "token" : "트구성품",
      "start_offset" : 1,
      "end_offset" : 5,
      "type" : "word",
      "position" : 6
    },
    {
      "token" : "구성",
      "start_offset" : 2,
      "end_offset" : 4,
      "type" : "word",
      "position" : 7
    },
    {
      "token" : "구성품",
      "start_offset" : 2,
      "end_offset" : 5,
      "type" : "word",
      "position" : 8
    },
    {
      "token" : "성품",
      "start_offset" : 3,
      "end_offset" : 5,
      "type" : "word",
      "position" : 9
    }
  ]
}
```