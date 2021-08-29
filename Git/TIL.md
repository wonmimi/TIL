> ## .gitignore에 추가한 파일명이 커밋 내역에 반영이 안될때
깃 캐시 제거 
```bash
git rm -r --cached .
```
혹시 이런 메세지가 나올시 
```bash
  * error: the following file has staged content different from both the
  file and the HEAD: ...  (파일명)
```
옵션 -rf 로 실행
```bash
git rm -rf --cached .
```
캐시 제거후, 변경사항 다시 커밋  
```bash
git add .
git commit -m "커밋 메세지"
```

[참고](https://stackoverflow.com/questions/11451535/gitignore-is-ignored-by-git)

> ## 슬랙 깃 알림 설정 
슬랙 알림 체널 메신저 창에서 
``` shell
#깃헙봇 추가 
/invite @GitHub

#깃헙 구독 추가 
/github subscribe [계정]/[저장소]

#깃헙 구독 해제 
/github unsubscribe [계정]/[저장소]
```

> ## 깃 커밋 메세지 규칙
[참고](https://blog.ull.im/engineering/2019/03/10/logs-on-git.html)

> ### 비밀번호 만료 메세지가 떴을 경우
push 했을떄 다음과 같은 에러메세지가 떴을 경우 (2021.08.14 이후 에러)
```shell
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
```
<img width="85%" alt="스크린샷 2021-08-20 오전 4 27 56" src="https://user-images.githubusercontent.com/79403710/131252139-e638335f-f0f8-4128-8475-8828f3cea964.png">

1) 깃헙에서 토큰 새로 생성후 
- <img width="80%" alt="스크린샷 2021-08-29 오후 10 34 31" src="https://user-images.githubusercontent.com/79403710/131252263-c0579088-3c95-4e02-8b9f-52387e3b118b.png">

2) 키체인 비밀번호로 재설정 
 - <img width="80%" alt="스크린샷 2021-08-29 오후 10 32 37" src="https://user-images.githubusercontent.com/79403710/131252166-745aa88e-4bb2-462f-84e3-012d37365917.png">

3) 정상적으로 작동 
- <img width="80%" alt="스크린샷 2021-08-20 오전 4 28 17" src="https://user-images.githubusercontent.com/79403710/131252323-46f71645-9529-42fd-8973-eb50496ecade.png">

- ref
  - [토큰 생성](https://firstquarter.tistory.com/entry/Git-%ED%86%A0%ED%81%B0-%EC%9D%B8%EC%A6%9D-%EB%A1%9C%EA%B7%B8%EC%9D%B8-remote-Support-for-password-authentication-was-removed-on-August-13-2021-Please-use-a-personal-access-token-instead)
  - [맥 키체인 설정](https://curryyou.tistory.com/403)
