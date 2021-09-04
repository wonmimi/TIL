## .gitignore에 추가한 파일명이 커밋 내역에 반영이 안될때
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

## 슬랙 깃 알림 설정 
슬랙 알림 체널 메신저 창에서 
``` shell
#깃헙봇 추가 
/invite @GitHub

#깃헙 구독 추가 
/github subscribe [계정]/[저장소]

#깃헙 구독 해제 
/github unsubscribe [계정]/[저장소]
```

## 커밋 메세지 
### 제목
- 타입
  * `기능(feat)`: 새로운 기능을 추가
  * `버그(fix)`: 버그 수정
  * `리팩토링(refactor)`: 코드 리팩토링
  * `형식(style)`: 코드 형식, 정렬, 주석 등의 변경(동작에 영향을 주는 코드 변경 없음)
  * `테스트(test)`: 테스트 추가, 테스트 리팩토링(제품 코드 수정 없음, 테스트 코드에 관련된 모든 변경에 해당)
  * `문서(docs)`: 문서 수정(제품 코드 수정 없음)
  * `기타(chore)`: 빌드 업무 수정, 패키지 매니저 설정 등 위에 해당되지 않는 모든 변경(제품 코드 수정 없음)
- 총 글자 수는 50자 이내며 마지막에 마침표(.)를 붙이지 않는다
- ref
  * [타입](https://udacity.github.io/git-styleguide/)
  * [자주쓰는 메세지](https://blog.ull.im/engineering/2019/03/10/logs-on-git.html)