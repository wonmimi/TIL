### .gitignore에 추가한 파일명이 커밋 내역에 반영이 안될때
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

### 슬랙 깃 알림 설정 
슬랙 알림 체널 메신저 창에서 
``` shell
#깃헙봇 추가 
/invite @GitHub

#깃헙 구독 추가 
/github subscribe [계정]/[저장소]

#깃헙 구독 해제 
/github unsubscribe [계정]/[저장소]
```