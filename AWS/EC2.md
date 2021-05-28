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

# EC2 접속
ssh wonmimi-webservice-aws (config에 등록한 Host명)
```

#### 2. 자바 기반 웹 애플리케이션 설정
1) java설치 
  ```zsh
  # 자바 8 설치 
  sudo yum install -y java-1.8.0-openjdk-devel.x86_6

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