 ## 정규식 [함수](https://www.php.net/manual/en/ref.pcre.php)
 - `preg_match` 정규식 패턴 일치 

 일치하면 1 아니면 0 return 
 ```php
 $digit_pattern = '/^[0-9]/i'; // 숫자 체크
$keyword_match = preg_match($digit_pattern, $keyword);

$type_patten = '/^[1-2]$/'; // 1아니면 2인지 체크
$type_match = preg_match($type_patten, $type);

$eng_pattern = '/^[a-zA-Z]+$/'; // 알파벳만

 ```

 <img width="604" alt="regex" src="https://user-images.githubusercontent.com/79403710/126596735-400c97c8-9005-41ab-b04f-de35fe1a1672.png">

- g : 전체에서 정규식이 맞는걸 찾는다, 전역검색 (global)
- i : 대소문자 구별안함 (case Insensitiv)
- m : 여러줄 검색 (multiline)

 ## 배열 관련 [함수](https://www.php.net/manual/en/ref.array.php)
 - `array_slice` 배열 자르기 <br>
 array_slice(배열 , 시작위치 , 길이 )
 ```php
    $complete_words = array_slice($array,0,$LIST_SIZE);
 ```
 - `array_sum` 배열 원소들의 합
```php
array_sum($array);
```
 - `array_search` 

 문자열의 키값(인덱스번호) return
 ```php
 (array_search('찾을문자열',$array) !== false)
 ```
 - `array_reverse` 역순 

array return
 ```php
  array_reverse($배열)
 ```
 - `array_push` 배열 마지막에 원소 추가 
 ```php
  array_push($array, $추가할배열);
 ```
            

 ## 문자열 관련 [함수](https://www.php.net/manual/en/ref.strings.php)
 -  `strpos` 문자열 탐색 문자열이 처음 나타나는 위치를 찾는 함수 <br>
   위치 값을 정수로 반환 ( 0 ~ , 없으면 false ) 하기때문에 `===` 로 비교 


 ```php
    if($strpos($문자열,$찾을 문자열) === false)
 ```
>  !== false  
 정확히 `false`이고 0이 아님 
  &nbsp; [참고](https://stackoverflow.com/questions/699507/why-use-false-to-check-stripos-in-php/699520)

  - `str_replace` 문자열 대체 

```php
// 바꿀문자열 -> 변경할 문자열
  $word = str_replace($바꿀문자열, $변경할문자열, $기존문자열);
```
- `str_ireplace` 대소문자 구분 X (매개변수 동일)
```php 
  $word = str_ireplace($바꿀문자열, $변경할문자열, $기존문자열);
```
 