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
![출력 화면](../img/laravel_local_setting.png)
