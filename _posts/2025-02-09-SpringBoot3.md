---
title: AWS와 DOCKER CI/CD 설정   
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

"JECKJECK" 프로젝트의 목표는 아래와 같다.   
1) SpringBoot + JSP + MySQL + BootStrap 프로젝트를 일정한 형태의 구조로 생성한다.   
2) 프로젝트 산출물을 정해진 형식에 맞추어 만든다.   
3) 로컬실행이 아닌, 스스로 운영의 환경을 만들어 배포한다.  
4) 운영의 환경은 AWS + Docker + CI/CD 로 진행한다.   
   
위의 목표 중 4번째 부분인 AWS + Docker + CI/CD에 대해 기록을 남기고자 한다.   

---

우선 AWS는 아마존 클라우드 웹 서비스이다.   
해당 서비스에서는 프리티어를 제공하고 있기에, 이를 활용하여 운영환경을 구축한다.   

프리티어계정에서 인스턴스 생성 후, 탄력적 IP를 할당 받는다.   
탄력적 IP란, 내가 생성한 서버의 공인IP라고 생각하면 이해가 쉽다.   

해당 서버에 보안그룹 설정 즉, 방화벽 설정을 통해 AWS콘솔에 접속하지 않고 외부 프로그램을 이용하여 접속이 가능하다.   
따라서, 필자는 프리티어 인스턴스 생성 및 IP설정은 다른 사이트에도 자세히 서술되어있기에 보안그룹 설정부터 설명하도록 한다.   

AWS 서버에 접속하는 프로그램으로는 MobaXterm을 이용하였다.(Potable Edition Free 버전)
=> https://mobaxterm.mobatek.net/   

해당 프로그램에서 아래와 같이 접속정보를 세팅한다.  
Remote Address : AWS의 Public IP   
빨간색으로 표시된 부분은 AWS생성시 받은 .pem key파일을 선택한다.    
![Image](https://github.com/user-attachments/assets/13514453-c350-4dc5-91bd-5b671e848f19)


아마 접속이 안될텐데, 이는 AWS 상에 방화벽 설정을 해주지 않았기 떄문이다.  
방화벽이란, 나의 서버(PC)의 보호막이라고 생각하면 조금 더 쉽게 이해가 될 듯하다.   
이러한 보호막에도 교류가 필요한데 이에 대해 규칙을 설정하는 것이 인바운드/아웃바운드 규칙이다.   
인바운드 규칙이란 외부에서 나의 서버에 접속을 허용하는 규칙을 의미하고 아웃바운드 규칙이란 나의 서버에서 외부에 대해 허용하는 규칙을 의미한다.   

아래와 같이 AWS상에서 인바운드 규칙을 생성한다.   
여기서 추가로 설명을 하자면, SSH가 외부 프로그램에서 접속을 하기 위한 규칙이다.  
![Image](https://github.com/user-attachments/assets/a6b8c794-beda-49da-833d-bbd15137b909)

위와 같이 세팅 후 접속하여 AWS상에 SpringBoot 파일을 배포하면 된다.  

sudo apt-get update
sudo apt-get install openjdk-17-jdk
java -version

---

아마 위에까지 진행한 것은 AWS상에 바로 SpringBoot 파일을 배포한 것이다.   
다만, 우리의 목표는 로컬과 동일하게 docker 위에 사이트를 동작시키는 것이므로 아래의 과정을 추가로 진행한다.   

우선, 필자는 로컬환경이 아래와 같다.   
1) 이클립스에서 기본 동작 확인   
2) docker desktop을 통하여 docker상에서 정상수행이 되었는지 확인.  
3) docker 운영 반영   

2)의 과정은 추후 필요하지 않을 듯 하지만, 혹시 에러가 발생할 수 있기에 해당 환경을 구축하였다.   

2)과정이 확인되면 docker hub의 Repository상에 image파일을 push하면된다.   

docker의 기본 개념은 vm과 비슷하지만 os와 무관하게 각 시스템별로 각기 다른 환경을 구축할 수 있다는 점에서 vm과 차이가 있다.   
여기서 image란 docker상에서 동작된 환경상태를 말한다. 
필자의 로컬환경을 기준으로 설명하면, springBoot 사이트가 dockerDesktop에서 정상 실행된 그 시점의 환경상태로 이해하면 된다.   

2)과정을 통하여 docker hub에 커밋했다면, 다시 AWS로 접속하기 바란다.   
접속하여, docker를 설치한다.   

```   
sudo apt update && sudo apt upgrade -y
sudo apt install -y ca-certificates curl gnupg

sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo tee /etc/apt/keyrings/docker.asc > /dev/null
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

sudo systemctl enable --now docker

sudo usermod -aG docker $USER

docker --version

docker run hello-world

```

도커 설치 후 아래의 명령어를 통해 hub에 커밋된 image를 가져온 후 설치하도록 한다.

```   
docker pull jaejae0015/jeckjeck:latest
docker run -d --name jeckjeck-container -p 80:8080 jaejae0015/jeckjeck:latest

```   

설치가 완료 되었다면 이제 퍼블릭ip:서비스포트 를 통해 접속을 확인하면 된다.

--- 

이제 여기서 한번 더 생각하여 발전해본다.   

docker를 쓰는 이유는 무중단 배포를 위하여 많이 사용한다. 
그런데 위의 방식은 컨테이너를 중지하고 해당 명으로 배포를 진행한다.   

그렇다면 기존 배포와 docker의 차이가 없는 것 아닌가..?   

따라서, 다음 CHATPTER에 무중단 배포를 구현함과 동시에 우리의 운영 최종 branch인 main에 커밋 시 자동 CI/CD가 되도록 만들어본다.


---