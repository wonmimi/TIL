### ps 
###### : 현재 돌아가고있는 프로세스 확인

#
> 검색 수집기 작업중 실시간 마이그레이션 스케줄링을 중단해야할 상황이 생겨 
> 실행중인 프로세스를 강제 종료 함.

- 프로세스 목록 확인
```bash
ps -ef 
```

- 특정 프로세스 확인
```bash
ps -ef | grep 프로세스명 
```
![출력 화면](../img/command_ps1.png)
![출력 화면](../img/command_pid.png)


- 프로세스 중단 시키기 
```bash
kill -9 PID
```
![출력 화면](../img/command_kill.png)
-9는 강제종료 


[참고](https://yang1650.tistory.com/110)