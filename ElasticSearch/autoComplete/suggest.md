# suggester

키워드 잘못 입력 , 결과 없을 경우 비슷한 키워드 변경해서 제시 하는 교정기능 제공

전방일치

- term suggest : 오타 교정 제안
- completion suggest : 검색어 예측 자동완성 제안 ✅
- phrase suggest : 추천 문장 제안
- context suggest : 추천 문맥 제안

### completion Suggest

- 글자 입력마다 결과 보여줘야 해서 응답 속도 중요.
- 내부에서 FST 작성하여 성능 최적화
- 데이터타입 : completion 설정한 인덱스 생성  [레퍼런스 (버전 6.8)](https://www.elastic.co/guide/en/elasticsearch/reference/6.8/search-suggesters-completion.html)

> 인덱스 생성

```sql
# 인덱스 생성
PUT test_index
{
   "mappings": {
    "_doc": {
      "properties": {
        "search_suggest": { // custom 
          "type": "completion"
        }
      }
    }
  }
}
```

> 데이터 입력 

```sql
PUT test_index/_doc/DA00TAtEsTTESTTEsTeS?refresh
{
    "search_suggest" : [
        {
            "input": "셀프빨래방"
        }
    ]
}
```

- input : 키워드 (필수값)
- weight : 우선수위 가중치값 - 선택

> 데이터 조회 
```sql
POST /test_index/_search?pretty
{
  "suggest": {
    "search_suggest": {
      "prefix": "빨",
      "completion": {
        "field": "search_suggest",
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


### 쿼리 - completion 속성

```sql
POST /auto_complete/_search?pretty
{
  "suggest": {
    "completion-suggest": {
      "prefix": "셀",
      "completion": {
        "field": "complate_suggest",
        "fuzzy": {
          "fuzziness": 0
        },
				"skip_duplicates":true,
        "size" : 7
      }
    }
  }
}
```

1. field : completion 타입의 필드명
2. prefix : keyword 전방 일치 탐색 - 주로 사용
3. fuzzy :  오타 무시 (length 설정)
    1. fuzziness
4. regex : 정규식
5. size : 결과 갯수  (default 5) 
6. skip_duplicates : 중복 제거 📌 
현재 log 데이터에 중복 키워드 많아서 필요한 옵션  (test keyword: 셀라도나이트  )

---

### 버전 6.8.5 (현재) → 7.* 업그레이드 이슈
suggester 관련 release Note 

[7.13.0](https://www.elastic.co/guide/en/elasticsearch/reference/current/release-notes-7.12.0.html)

> 보안 업데이트 
- A document disclosure flaw was found in the Elasticsearch suggester and profile API when Document and Field Level Security are enabled. The suggester and profile API are normally disabled for an index when document level security is enabled on the index. Certain queries are able to enable the profiler and suggester which could lead to disclosing the existence of documents and fields the attacker should not be able to view. All versions of Elasticsearch before 6.8.15 and 7.11.2 are affected by this flaw. You must upgrade to 6.8.15 or 7.11.2 to obtain the fix 📌

[7.3.0](https://www.elastic.co/guide/en/elasticsearch/reference/current/release-notes-7.3.0.html)

> - Fix suggestions for empty indices 
- Skip explain phase when only suggestions are requested

[7.2.0](https://www.elastic.co/guide/en/elasticsearch/reference/current/release-notes-7.2.0.html)

> - Search as you type fieldmapper

[7.0.0](https://www.elastic.co/guide/en/elasticsearch/reference/current/release-notes-7.0.0.html)

> - Fix threshold frequency computation in Suggesters
- Make Geo Context Mapping Parsing More Strict 
- Remove the ability to index or query context suggestions without context

---

[참고](https://deviscreen.tistory.com/29)

[참고 - 2 ](https://www.skyer9.pe.kr/wordpress/?p=1101)

---

