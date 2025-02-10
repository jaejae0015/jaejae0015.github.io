---
title: AWS와 DOCKER CI/CD 설정 2   
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

> "JECKJECK" 프로젝트의 목표는 아래와 같다.   
> 1) SpringBoot + JSP + MySQL + BootStrap 프로젝트를 일정한 형태의 구조로 생성한다.   
>2) 프로젝트 산출물을 정해진 형식에 맞추어 만든다.   
>3) 로컬실행이 아닌, 스스로 운영의 환경을 만들어 배포한다.  
>4) 운영의 환경은 AWS + Docker + CI/CD 로 진행한다.   
>   
>위의 목표 중 4번째 부분인 AWS + Docker + CI/CD에 대해 기록을 남기고자 한다.   

저번에 작성하였던 글에 이어, 무중단 배포를 구현함과 동시에 우리의 운영 최종 branch인 main에 커밋 시 자동 CI/CD가 되도록 만들어본다.   

---   

무중단 배포는 docker-compose와 git hub action을 이용하여 구현한다.

우선, 다시 AWS에 접속한다.   
그 후, 아래와 같이 docker-compose설치한다.   

```   
sudo curl -L "https://github.com/docker/compose/releases/download/v2.18.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version

```   

docker-compose파일은 아래와 같다.   

```
version: '3'
services:
  app:
    image: jaejae0015/jaejaeproject:latest  #github에 커밋된 image파일명명
    container_name: jeckjeck                #컨테이너 중지 및 생성을 위해 지정한다.
    restart: always
    ports:
      - "9909:9909" 
    environment:
      - SPRING_PROFILES_ACTIVE=prod
```

--- 

우선 CI/CD의 의미는 아래와 같다.


CI/CD 구축 전 Git Repository의 Setting에 접속 후, 변수를 세팅하자.   
![Image](https://github.com/user-attachments/assets/e525637f-42f7-4cbd-8ec6-df28501d6f27)


참고로, 각각의 의미는 아래와 같다.
EC2_SSH_PRIVATE_KEY : aws .pem 키 값
DOCKER_USERNAME : docker hub 계정
DOCKER_HUB_TOKEN : docker hub 개인 토큰 값
AWS_EC2_HOST : 탄력적 ip 주소   


GIT의 본인 Repository에 들어가서 아래의 메뉴에 접속하자.   
(필자는 Java Maven 프로젝트이다.)   
![Image](https://github.com/user-attachments/assets/d847e7c3-2ed7-4dae-9b90-4d36d208893d).

생성을 진행하면 자동으로 파일을 만들어준다.  
해당 파일을 아래와 같이 수정하여 docker에 CI/CD를 설정하자.   
```
# This workflow will build a Java project with Maven, and cache/restore any dependencies to improve the workflow execution time
# For more information see: https://docs.github.com/en/actions/automating-builds-and-tests/building-and-testing-java-with-maven

# This workflow uses actions that are not certified by GitHub.
# They are provided by a third-party and are governed by
# separate terms of service, privacy policy, and support
# documentation.

name: Java CI/CD with Maven

on:
  push:
    branches: [ "main" ]
  pull_request:
    branches: [ "main" ]

jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read

    steps:
    - uses: actions/checkout@v4

    - name: Set up JDK 17
      uses: actions/setup-java@v4
      with:
        java-version: '17'
        distribution: 'temurin'
        cache: maven

    - name: Build with Maven
      run: mvn -B package --file pom.xml

    - name: Build without Tests
      run: mvn clean package -DskipTests

    - name: Log in to DockerHub
      uses: docker/login-action@v2
      with:
        username: ${{ secrets.DOCKER_USERNAME }}
        password: ${{ secrets.DOCKER_HUB_TOKEN }}

    - name: Build Docker Image
      run: |
        docker build -t ${{ secrets.DOCKER_USERNAME }}/jaejaeproject:latest .
        docker push ${{ secrets.DOCKER_USERNAME }}/jaejaeproject:latest

    - name: SSH to Server and Deploy
      uses: appleboy/ssh-action@v0.1.6
      with:
        host: ${{ secrets.AWS_EC2_HOST }}
        username: ubuntu
        key: ${{ secrets.EC2_SSH_PRIVATE_KEY }}
        script: |
          docker pull jaejae0015/jaejaeproject:latest  # 커밋 SHA를 활용한 태그 사용
          docker-compose -f /home/ubuntu/docker-compose.yml pull
          docker-compose -f /home/ubuntu/docker-compose.yml up -d --no-deps jeckjeck  # 'jeckjeck' 서비스만 갱신
          docker image prune -f
```

위와 같이 진행하면 무중단 CI/CD 파이프라인 구축완료이다.   

---   