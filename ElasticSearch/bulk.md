## 대량데이터 넣기 bulk

 인덱스에 기존 대량 데이터를 넣으려고 할때 (ex 6만건)

[레퍼런스](https://www.elastic.co/guide/en/elasticsearch/client/php-api/current/indexing_documents.html) 를 보고 넣었더니 

```bash
{"error":{"root_cause":[
{"type":"action_request_validation_exception",
"reason":"Validation Failed: 1: type is missing;"}],
"type":"action_request_validation_exception",
"reason":"Validation Failed: 1: type is missing;"},"status":400}
```

에러가 자꾸뜸  [검색](http://ftyjtk.blogspot.com/2018/11/how-do-you-send-bulk-inserts-with-no.html) 해보니 "_type" 을 넣어 줘야함 

> PHP 코드 
```php
for($i = 1; $i < 3; $i++) {
	$params['body'][] = [
	            'index' => [
	                '_index' => $index,
	                '_type' => '_doc' // 📌
	          ]
	        ];
	    
	        $params['body'][] = [
	            'keyword'     => 'my_value '.$i,
	            'score' => $i
	        ];
	
	$responses = $client->bulk($params);
}

$responses = $client->bulk($params);
```

- _id 값을 지정하지않으면 _id값 임의로 생성