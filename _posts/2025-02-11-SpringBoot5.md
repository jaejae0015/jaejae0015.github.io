---
title: AWS와 DOCKER CI/CD하면서 발생한 에러 정리
author:
  name: jaejae0015
  link: https://github.com/jaejae0015
date: 2025-02-09
categories: [jaejae0015, Record]
math: true
mermaid: true
render_with_liquid: false
pin: false
comments: true
#image:
#  src: /commons/devices-mockup.png
#  width: 800
#  height: 500
---

1) AWS 서버가 실행되다가 자동으로 딜레이되고 멈추는 경우   
=> SWAP 메모리 설정을 통해 해결하자.   

```
# swapfile 메모리를 할당
sudo dd if=/dev/zero of=/swapfile bs=128M count=16

# swapfile 권한 세팅 (READ, WRITE)
sudo chmod 600 /swapfile

# swap 공간 생성 (Make swap)
sudo mkswap /swapfile

# swap 공간에 swapfile 추가하여 즉시 사용할 수 있도록
sudo swapon /swapfile

# /etc/fstab vi 에디터 열기
sudo vi /etc/fstab

# 파일의 맨 끝 다음줄에 아래 명령어 작성
/swapfile swap swap defaults 0 0

# 메모리 확인
free

```

2) AWS에 SpringBoot 배포 시, DB에러  
=> AWS내부에 DB를 만들자. 
```
docker run -d \
  --name mysql-container \
  -e MYSQL_ROOT_PASSWORD=my-secret-pw \
  -e MYSQL_DATABASE=mydatabase \
  -e MYSQL_USER=myuser \
  -e MYSQL_PASSWORD=mypassword \
  -p 3306:3306 \
  --restart unless-stopped \
  mysql:latest

```

-d : 백그라운드 실행 (Detached mode)
--name mysql-container : 컨테이너 이름 지정 (mysql-container)
-e MYSQL_ROOT_PASSWORD=my-secret-pw : root 계정 비밀번호
-e MYSQL_DATABASE=mydatabase : 자동 생성할 데이터베이스 이름
-e MYSQL_USER=myuser : 새로 생성할 사용자 이름
-e MYSQL_PASSWORD=mypassword : 새 사용자 비밀번호
-p 3306:3306 : MySQL의 기본 포트(3306) 개방
--restart unless-stopped : 컨테이너가 자동 재시작되도록 설정


```
docker ps

docker exec -it mysql-container mysql -u root -p

SHOW DATABASES;

```

---   