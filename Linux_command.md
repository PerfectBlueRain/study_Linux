
#### 리눅스 정보와 통계
- uname -a  // 커널정보등 시스템 정보
- uname -m  // arch

#### user & group
- /etc/passwd : " root:x:0:0:root:/root:/bin/bash "
- /etc/shadow : " root:$1$9L2L0oTwd:12751:0:99999:7 : : "
- /etc/group :

- who
- last | more

- 계정생성
```bash
#ids 계정 생성
$> groupadd -g 501 ids
$> useradd -u 501 -g 501 -d /app/ids -s /bin/bash ids
$> cat /etc/passwd
```

#### 시작스트립트
- ~/.bashrc : alias / function설정
- ~/.bash_profile : user특수한 환경설정 / startup

#### vim설정
- ~/.vimrc

---

### 파일안에 특정단어가 나온 라인수
$> cat {파일명} | grep {단어} | wc -l
$> cat /var/log/messages | grep segfault | wc -l

### System환경 파악
df -hT
free -g

### iptables
 iptables -nL // iptables로 오픈된 포트확인 / dpt:포트 라고 적혀있는부분이 오픈
service iptables stop
sudo iptables -I INPUT 1 -p tcp --dport 9200 -j ACCEPT

-포트변경후 iptables 재시작
$ /etc/init.d/iptables restart 또는
$ /etc/rc.d/init.d/iptables start

### netstat
- http://khie74.tistory.com/1169521441
netstat -nap | grep LISTEN //LISTEN중인 포트를 표시하기
nc -z 호스트주소 포트 //상대방 포트가 열려 있는지를 확인하는 방법
nc 호스트주소 -z 시작포트-끝포트 //특정 머신의 포트 범위를 지정하여 열린 포트를 확인하기
---

### 로그에서 40x로그
$> tail -f /app/log/ds/accesslog/access.log | grep -E '\" 40.'

### 로그분석을 특정 키워드가 들어가 라인을 리다이렉트
$> grep segfault /var/log/messages > /app/log/msg.log

### ssh timeout설정
$> export TMOUT=600  (10분)
$> unset TMOUT   (이러면 안끊김)
이런 경우는 아래와 같이 SSH 의 구성파일을 수정 하시거나,
$ vi /etc/ssh/sshd_config

ClientAliveInterval 600
ClientAliveCountMax 3

(600 초 x 3) = 30분간 세션 유지

시스템 혹은 개인계정에 대한 프로파일에 timeout 시간을 정의 해 주면 됩니다.

$ vi ~/.bash_profile
$ vi /etc/profile

TMOUT=600    (10분 타임아웃)
입력 후 저장

바로 적용하려면,

$ source ~/.bash_profile
$ source /etc/profile

혹은
export TMOUT=600


### 접속한 사용자 정보가 길어서 more로
- last | more   
- who

### cron
- tail -100f /var/log/cron : cron이 잘 진행되고 있는지 로그를 확인
- crontab -e : root권한없이 구가용자의 cron등록

### sar
sar : 설치및 사용 http://bencane.com/2012/07/08/sar-sysstat-linux-performance-statistics-with-ease/

### ab
ab : 아파치벤치 http://httpd.apache.org/docs/2.2/ko/programs/ab.html

### tip
```
tail -f filname.txt //실시간으로볼수있다.
date -u // 시간날짜를 UTC기준으로 보여준다.
mkdir -p /app/log/ds/script //

find /app/ds/data/cache/hub/vsm -exec touch {} \;    //리눅스 파일날짜 일괄변경  http://knamhun.blogspot.kr/2009/03/find.html
find ./ -name '*.so' //현제 디렉터리에서 찾는다.
```

### vi /etc/profile        // 리눅스의 환경설정파일  (source /etc/profile )
Icon
export JAVA_HOME=/usr/java/jdk1.6.0_35
export PATH=$PATH:$JAVA_HOME/bin
export CLASSPATH=$JAVA_HOME/jre/ext:$JAVA_HOME/lib/tools.jar
export HADOOP_HOME=/home/root/hadoop-2.5.2

### vi /etc/hosts : 여기에 ip와 host name을 매핑해서 쓸수있다.
### vi /etc/sysconfig/network  // host네임 설정
Icon
[root@namenode ~]# vi /etc/hosts
192.168.56.101  namenode        # Added by NetworkManager
192.168.56.102  datanode1
192.168.56.103  datanode2

### scp
- scp /etc/profile datanode1:/etc/

### netstat -nlp
```
Icon
roto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name
tcp        0      0 0.0.0.0:3306            0.0.0.0:*               LISTEN      1189/mysqld
 tcp        0      0 0.0.0.0:21              0.0.0.0:*               LISTEN      1324/vsftpd
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      1015/sshd
tcp        0      0 127.0.0.1:6011          0.0.0.0:*               LISTEN      1838/1
tcp6       0      0 :::80                   :::*                    LISTEN      1634/apache2
tcp6       0      0 :::22                   :::*                    LISTEN      1015/sshd
```

### ssh : chmod 600 testkey.pem

### 명령의 결과를 저장하기 위해 `( back quarter)를 사용했다.

### ‘(single quarter)과 혼동 할 수 있으므로 주의 하도록 한다.


### 서버용량확인 du / df
```
df : 디스크의 남은 용량을 확인
$> df -k : 킬로바이트 단위로 현재 남은 용량을 확인
$> df -m : 메가바이트 단위로 남은 용량을 왁인
$> df -h : 보기 좋게 보여줌
$> df . : 현재 디렉토리가 포함된 파티션의 남은 용량을 확인

du : 현재 디렉토리에서 서브디렉토리까지의 사용량을 확인
$> du -a : 현재 디렉토리의 사용량을 파일단위 출력
$> du -s : 총 사용량을 확인
$> du -h : 보기 좋게 바꿔줌
$> du -sh * : 한단계 서브디렉토리 기준으로 보여준다.
```

### Linux 압축
```
.zip
 압축 $ zip -r [압축파일명.zip] [압축할 파일/디렉토리]
압축 해제 $ unzip [압축파일명.zip]
.tar
압축 $ tar cf [압축파일명.tar] [압축할 파일/디렉토리]
압축 해제 $ tar xf [압축파일명.tar]
 tar.gz
압축 $ tar zcf [압축파일명.tar.gz] [압축할 파일/디렉토리]
압축 해제 $ tar xfz [압축파일명.tar.gz]
 .tar.bz2
압축 $ tar jcf [압축파일명.tar.bz2] [압축할 파일/디렉토리]
압축 해제 $ tar xfj [압축파일명.tar.bz2]
.tar.xz - 이중으로 압축을 풀어야 합니다.
 압축 해제 (.xz 압축 해제 -> tar 압축 해제)
$ xz -d [압축파일명.tar.xz]
$ tar -xf [압축파일명.tar]
```
