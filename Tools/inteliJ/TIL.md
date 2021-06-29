
>Too many characters in character literal
- 일반적으로 작은 따옴표 즉 '' 2 바이트 
  => 하나의 문자 인 문자 리터럴을 지정하는 데 사용됩니다.
- 문자열이나 여러 문자를 사용하려면 "" 큰 따옴표를 사용합니다.


> cannot access class...와 같은 경고와 함꼐 같은패키지에 클래스를 못찾는 경우

- 같은 패키지 안에 있는 클래스를 사용했는데 인텔리제이가 빨간 줄 뜸
- 빌드를 돌려보면 아무런 문제가 없다. 이런 경우엔 인텔리제이 캐시에 잘못된 파일이 저장되어 있을 수 있다.
- File - Invalidate Caches -> Restart를 실행하면 inteliJ 종료되고 재실행 되면서 해결 <br>
  * [참고](https://johngrib.github.io/wiki/intellij/)
