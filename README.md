# LinuxStudy

## 리눅스

* 개발자가 직접적으로 하드웨어(CPU,RAM)를 제어하는건 소프트웨어개발자에겐 어렵다.

* 개발자는 하드웨어를 제어하는 OS에게 명령을 내리면 간접적으로 제어가 가능하다

* 개발자와 OS사이에는 shell이라는 언어를 사용(OS마다shell언어가 다름)

### SSH(Secure Shell)

원격 호스트에 접속하기 위해 사용되는 보안 프로토콜 (보통 22번 포트)
 
### RSA인증방식

이전에 `대칭키 암호화방식`은 하나의 비밀키로 암호화,복호화를 수행하는 방식이다 이 방식은 중간에 비밀키가 담긴 패킷을 가로채 암호화 정보를 해독할 수 있다는 문제점이 있다.

`공개키 암호화방식`중 RSA는 암호화+전자서명까지 가능한 알고리즘이다


```
친구 -------------------------------------------- 나
public key            |                  public key(공개키)
private key           |                 private key (비밀키)
                      |
                      C

```

위 상황에서 친구가 나에게 데이터를 암호화 시켜서 보내고 싶으면
나의 public key로 데이터를 암호화 시키면 중간에 C가 가로채어도
데이터를 열어볼 방법이 없다

암호화된 데이터는 나의 private key로만 복호화가 가능하다.

### `만약 C가 가로채서 패킷을 버리고 새로운 패킷을 만들어 B의 public key로 암호화한다면??`

이때 전자서명 개념이 들어가는데, 친구쪽에서 암호화시킨 데이터를 한번더 친구의 비밀키로 암호화시킨다. 이게 전자서명의 개념인데,

이러면 C가 중간에 가로채도 친구의 비밀키를 가지고 있지 않기때문에 나에게 패킷이 넘어왔을때 친구의 공개키로 열리지 않기때문에 데이터 위변조가 걸린다. 

## 리눅스 명령어

* pwd 현재 위치 

* ls 현재 위치한 디렉토리 안에 있는 폴더,파일을 보여줌

    * ls -l 자세히 보고 싶을때 (앞에 d가 붙으면 폴더, -는 파일)
    * ls --help ls의모든 명령어의 사용법을 알려준다
    * ls --all 숨김파일포함 모든 파일을 보여준다
    * ls -al (ls -l+ls --all)가 합쳐진 명령어
* mkdir 폴더생성시 사용
* touch 파일생성시 사용
* cat [파일명] 파일내용볼 수 있음
* rm 파일삭제
* rm -r 폴더삭제
* cp [복사파일이름] [새로운이름]
* mv [옮길파일이름] [경로]
* mv [파일명] [새로운파일명] 파일이름도 바꿀 수 있다
* ln -s [바로가기대상파일이름][바로가기파일이름] 바로가기폴더 생성
* netstat -nlpt 포트번호 보는 법
* lsb_release -a 우분투버전,코드명 확인
* 서비스 종료,시작
    * sudo systemctl stop [프로세스이름]
    * sudo systemctl start [프로세스이름]
* ps -ef 프로세스들에 대한 정보 보여줌
* kill [pid] 프로세스 강제종료
    * kill명령어를 당한 프로세스 재시작 명령어
    * sudo systemctl restart [프로세스이름]
* ps -ef에서 특정 프로세스의PID만 꺼내는법
    * ps -ef | grep [프로세스이름]|grep -v grep| awk '{print $2}'
    * 여기에 백틱넣어서 프로세스 kill가능
* vi [파일명] vi에디터로 들어가기 
* sudo passwd root 최고관리자 비밀번호 변경하기
* sudo chmod 777 권한부여 (r:4, w:2, x:1)
* sudo chown [소유자]:[그룹] [파일명] 그룹에 권한부여가능
* sudo find / [파일명] 파일찾기
* sudo apt-cache search jdk | grep openjdk-11 --jdk중에서도 11버전만 보고싶을때
* sudo apt install [파일명]
* tail -f nohup.out -- 로그 모니터링 하는 법


경로 이동시 절대경로로 갈 경우 맨 앞에 '/'를 붙여줘야 한다

데몬프로세스란 항상 실행중인 프로그램

## 배포하기
1 ec2서버에서 clone

2 gradlw에 실행권한주기

3 jdk설치하기

4 mysql설치
>sudo apt install mysql-server-8.0

5 gradlw로 프로젝트를 jar파일로 변경
>./gradlw build  

앞에 ./를 붙여야됨 안붙이면 환경변수를 찾는다

```
jar {
    enabled = false
}
```
build.gradle에 넣어줘야 jar파일 한개만 생성

6 java로 변경된 jar파일 실행
> java -jar blog-0.0.1-SNAPSHOT.jar

7 백그라운드로 실행하기 
>nohup java -jar blog-0.0.1-SNAPSHOT.jar &
