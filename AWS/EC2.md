### 인스턴스 
- 생성중 보안그룹 할때 ssh접속시 내 IP로 지정 (다른 장소 접속시, 해당장소의 IP ssh 규칙 추가 적용)
  * 생성한 pem키로 인스턴스 접근 (유출 조심)
- 인스턴스는 정지후 재시작 하면 ip 도 변동됨
- 고정 ip = Elastic IP (EIP, 탄련적 ip) 생성하여 연결해준다
  * 생성후 EC2에 바로 연결하지않으면 비용이 발생하므로 바로연결 
  * 사용할 인스턴스가 없을때에도 탄력적 IP를 삭제 해야한다 

#### 1. 로컬에서 EC2 서버 접속
pem 키 파일을 자동으로 읽을수 있도록 ~/.ssh/ 디렉토리로 이동 
```zsh
# .ssh 로 복사 
cp [pem파일 위치] ~/.ssh/

# pem키 권한 변경
chmod 600 ~/.ssh/키이름

# config 파일 수정 
vi ~/.ssh/config 

#  config 파일 내용 
  Host wonmimi-webservice-aws (원하는 서비스명)
  HostName 탄련적 IP
  User ec2-user
  IdentityFile ~/.ssh/pem키 이름


# config 권한 변경
chmod 700 ~/.ssh/config
```

EC2 접속
```zsh
ssh wonmimi-webservice-aws (config에 등록한 Host명)
```
![접속 성공시](../img/ec2-hostname-before.png)

#### 2. 아마존 리눅스 서버 생성시 필수 설정 
\+ 자바 기반 웹 애플리케이션
1) java설치 
  ```zsh
  # 자바 8 설치 
  sudo yum install -y java-1.8.0-openjdk-devel.x86_6
  # 자바 11인경우
  sudo amazon-linux-extras install java-openjdk11

  # 인스턴스 자바 버전 설정
  sudo /usr/sbin/alternatives --config java

  # 사용하지않는 버전이 있을경우 삭제 
  sudo yum remove java-1.7.0-openjdk (자바 버전)
  ```
2) 타임존 변경 
  ```zsh
    # 기본 UTC -> KST 변경
    sudo rm /etc/localtime
    sudo ln -s /usr/share/zoneinfo/Asia/Seoul /etc/localtime
  ```
3) 호스트네임 변경
```zsh
  # config 파일 수정
  sudu vim /etc/sysconfig/network

  #HOSTNAME 추가 (또는 변경)
  NETWORKING=yes
  NOZEROCONF=yes
  HOSTNAME=wonmimi-webservice-aws
```

 \** amazone ami2 인 경우, [참고](https://vkein.tistory.com/entry/%EC%95%84%EB%A7%88%EC%A1%B4-EC2-%EC%B4%88%EA%B8%B0-%EC%84%A4%EC%A0%95%ED%95%98%EA%B8%B0)
```zsh
  sudo hostnamectl set-hostname 등록할 호스트명
```
 서버 재부팅 (후 변경된 HOSTNAME 확인)
```zsh
  sudu reboot
```
  * 재접속 하여 hostname 확인 
![접속 시](../img/ec2_hostname_2.png)
  * /etc/hosts 에 hostname 등록
```zsh
  sudo vim /etc/hosts

  # hostname 작성
  127.0.0.1   localhost localhost.localdomain localhost4 localhost4.localdomain4
  ::1         localhost6 localhost6.localdomain6

  127.0.0.1       wonmimi-webservice-aws
```
  * 등록 확인 
  ```zsh
    curl wonmimi-webservice-aws (등록 HOSTNAME)
  ```
  * 80포트 접근에러가 뜨면 등록 OK
  ![curl](../img/ec2_curl.png)

  ```zsh
  curl: (7) Failed to connect to wonmimi-webservice-aws port 80: Connection refused
  ```
  아직 80포트로 실행된 서비스가 없음. curl 포스트 실행은 OK