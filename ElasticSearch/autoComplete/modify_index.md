# 인덱스 설정을 바꾸고싶다면 ?

- setting 수정

```sql
# OPEN
POST auto_complete_wm_ngram/_open

# 수정 ex) 
PUT auto_complete_wm_ngram/_settings
{
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
}

# 닫기
POST auto_complete_wm_ngram/_close
```

close 안하고 변경하면 에러메세지 

```sql
"type": "illegal_argument_exception",
    "reason": "Can't update non dynamic settings [[index.analysis.tokenizer.ngram_tokenizer.type, index.analysis.tokenizer.ngram_tokenizer.max_gram, index.analysis.analyzer.ngram_analyzer.tokenizer, index.analysis.tokenizer.ngram_tokenizer.min_gram, index.analysis.tokenizer.edge_ngram_tokenizer.type, index.analysis.tokenizer.edge_ngram_tokenizer.max_gram, index.analysis.analyzer.edge_ngram_analyzer.tokenizer, index.analysis.analyzer.edge_ngram_analyzer.filter, index.analysis.analyzer.ngram_analyzer.filter, index.analysis.tokenizer.edge_ngram_tokenizer.min_gram]] for open indices [[auto_complete_wm_ngram/jcnL6LT-TxGotllJ_9LjMQ]]"
  },
```

- mapping 수정  (항목추가 , 기존꺼 변경은 aliases 변경으로)

```sql
PUT auto_complete_wm_ngram/_mapping/_doc
{
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
```