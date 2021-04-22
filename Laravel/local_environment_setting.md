### 라라벨 로컬 환경 컴포저 셋팅
- sfrp.json 파일 수정 (tool :vsCode)
```json
"uploadOnSave": false,
"downloadOnOpen": false,
    "ignore": [
        ".vscode",
        ".git",
        "vendor",
        ".DS_Store"
    ]
```

=> 로컬에 실행하는 것과 별개로 , sftp 업로드 되지 않도록 / 보통 uploadOnSave = false 로 지정하는게 확실ㅎ 


- 터미널 에서 composer install
- 작업 워크스페이스에서 명령어 실행

```bash
php arisan serve
```

- 출력된 로컬 ip 접속해서 실행
![출력 화면](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/30b30612-681d-4e93-86d8-b0adaa9e513a/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20210422%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20210422T143417Z&X-Amz-Expires=86400&X-Amz-Signature=0ff269732a9d82168db8a04c49327b9b46fdb79bc8b7f51db7188f1ce556fe13&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)
