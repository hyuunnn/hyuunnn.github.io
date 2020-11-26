---
layout: post
title: "IDA Plugin FIRST 서버 구축"
description: "FIRST"
date: 2018-01-05
tags: [IDA Plugin, cisco talos, FIRST]
comments: true
share: true
---


### FIRST란?

cisco talos에서 개발한 함수 유사도 측정 플러그인이다.

매칭하는 부분을 간단하게 분석해본 결과 ESP나 EBP 같은 특정 코드를 정규표현식으로 필터링을 거친 후에 intersection(교집합)을 이용해 판별을 한다.

그 외에도 주소 값이 같다라든지 등등 비교하는 부분이 있었던걸로 기억한다.

#### 설치 방법

FIRST Plugin은 IDA 6.9 이상부터 호환이 가능하고 IDA 7.0버전일 경우에는 branch를 dev로 바꾼 후에 설치해야한다. (2018년 1월 5일 기준 아직 dev버전)

설치 방법은 <a href="https://first-plugin-ida.readthedocs.io/en/latest/installing.html">FIRST Tutorial</a>에 자세하게 나와있다.

![FIRST](/assets/images/First/First-01.png)

First는 기본적으로 <a href="http://first-plugin.us/">first-plugn.us</a>에서 apikey를 뽑아서 사용 할 수 있다.

하지만 서버를 구축하는 것 또한 가능하다. <a href="https://github.com/vrtadmin/FIRST-server">FIRST-server</a>

FIRST-server는 python2.7 코드이기 때문에 python2.7은 우분투에 자동으로 깔려있지만 없다면 깔아야 한다.
```
git clone https://github.com/vrtadmin/FIRST-server
FIRST-server/install에 있는 requirements.txt 설치 (pip2 install -r requirements.txt)
apt-get install mysql-server
apt install mongodb-clients
apt install mongodb-server
mongo 했을 때 이상한 에러 뜨면 
sudo rm /var/lib/mongodb/mongod.lock
sudo service mongodb restart
vi run.sh로 열은 후에 코드를 보면 환경변수로 되어있다.
```

![FIRST](/assets/images/First/First-05.png)
```
export MYSQL_HOST=127.0.0.1
export MYSQL_USER=root
export MYSQL_PASSWORD=<password> 설정과 밑에 migrate 하는 코드 manage.py 경로에 맞게 설정한다.
mysql -uroot -p<password>으로 접속 후 create database first_db; first_db라는 이름으로 생성한다.
ssl 에러가 뜬다면
sudo mkdir /etc/apache2/ssl
sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/apache2/ssl/apache.key -out /etc/apache2/ssl/apache.crt
옵션을 간단하게 설정해준다.
disallowedhost가 뜬다면
FIRST-server 내부에 있는 setting.py에 ALLOWED_HOSTS = '*'로 수정해주면 된다.
그 후에 python manage.py runserver 0.0.0.0:port로 열고 윈도우 환경에서 열면 정상적으로 열린다.
```

![FIRST](/assets/images/First/First-03.png)

위 에러 외에도 django나 linux에서 에러가 뜬다면 구글링을 통해 해결해야한다. 위 에러들은 내가 구축해보면서 나왔던 에러들이다.

구축을 완료한 후에 서버를 열고 들어가면 사이트가 정상적으로 나올 것이다.

하지만 FIRST는 google token을 이용해 회원가입을 하고 db에 등록한 함수를 계정으로 관리하기 때문에 token 설정도 해줘야한다.

<a href="https://console.developers.google.com/">google api token</a>

![FIRST](/assets/images/First/First-04.png)

위 사진과 같이 URI는 임의의 주소를 적어둔다. IP를 입력했을 경우 정상적으로 작동되지 않았다.

생성한 후에 JSON 다운로드 버튼을 눌러 받아지는 json 파일 이름을 google_secret.json으로 변경하고

cd /usr/local/etc에 google_secret.json 파일을 넣어주면 된다.

![FIRST](/assets/images/First/First-02.png)

마지막으로 hosts 파일을 위 사진과 같이 redirect하게 수정해준 다음 접속하면 회원가입까지 정상적으로 동작한다.

![FIRST](/assets/images/First/First-06.png)

만약 분석 결과가 안나오거나 이상하게 나온다면 엔진 세팅을 하지 않은 문제 때문이다.

![FIRST](/assets/images/First/First-07.png)

맨 처음에 있었던 FIRST-server readthedocs 페이지에 있는 설정 방법을 참고하면 된다.