---
layout: post
title: "mongodb 에러"
description: ""
date: 2023-08-10
tags: []
---

mongodb를 <a href="https://www.mongodb.com/docs/v5.0/tutorial/install-mongodb-on-ubuntu/">설치</a>하고 개발 파트에서 만든 백엔드 코드를 돌렸는데 에러가 발생했다.

![0](/assets/images/mongodb-error/0.png)

`sudo service mongod status` 명령어를 확인하면 `failed` 상태로 되어있었다.

![1](/assets/images/mongodb-error/1.png)

<a href="https://spiralmoon.tistory.com/entry/MongoDB-tmpmongodb-27017sock-error-operation-not-permitted">링크</a>를 참고하여 `/var/log/mongodb/mongodb.log` 파일을 확인해보니 `mongodb-27017.sock` 파일 관련 에러가 존재했다.

파일 삭제 후 재시작하니까 해결됐다.

```console
sudo rm -rf /tmp/mongodb-27017.sock
sudo systemctl stop mongod
sudo systemctl start mongod 
```

![2](/assets/images/mongodb-error/2.png)

그외에 잡다한 에러들은 충돌 가능성도 있고, 이유를 모르는 문제라서 깔끔하게 삭제 후 다시 설치하는 것을 추천한다.

```console
dpkg --list |grep mongo
sudo apt-get --purge remove 패키지명
```

### 참고 링크

https://hoyeong-rithm.tistory.com/69

<a href="https://velog.io/@tyufjvbn2/%EC%9A%B0%EB%B6%84%ED%88%AC-Mongo-DB-%EC%A4%91%EB%B3%B5-%EC%84%A4%EC%B9%98%EC%8B%9C-%EC%98%A4%EB%A5%98">우분투 Mongo DB 중복 설치시 오류</a>
