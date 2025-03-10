---
title: Tomcat에 대해서 다시 알아보기
author:
  name: jaejae0015
  link: https://github.com/jaejae0015
date: 2025-03-10
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

오늘은 WAS와 Tomcat에 대해 상세하게 알아보고자 한다.  

개발을 하면서 WAS서버에 대해서 많이 접하기 때문에 익숙할 것이다.   
다만, 안의 내부 구조가 어떻게 되어 동작되는지에 대해서는 정확하게 모를 수 있어 정리해 보고자 한다.   


오늘 설명하고자 하는 기준은 Apache Tomat이다.  
참고해서 아래 내용을 보면 좋겠다.  


---   

우선, WebServer와 WAS서버에 대한 개념을 알아야한다.   

WebServer는 정적인 컨텐츠(HTML, CSS, JavaScript 등..)에 대한 처리만 가능한 서버로 Static서버라고도 말한다.   
이에 반해, WAS(Web Application Server)의 경우는 DB의 데이터를 읽어와 동적인 컨텐츠(JSP, PHP 등...)를 렌더링 하여 전달 가능하다.   

대표적으로 사용하는 WebServer에는 Apache(Httpd), NginX 등이 있으며, WAS에는 Tomcat, Node.js, JBoss, JEUS 등이 존재한다.   

이러한 두가지의 서버를 동시에 사용하는 이유는 아래와 같다.
1) WebServer는 로드밸런싱 처리와 정적인 컨텐츠를 빠르게 처리하여 제공하기 위해 알맞다.
2) 동적인 데이터처리를 위해서는 WAS를 사용해야한다.  


설명을 진행하는 과정에서 로드밸런싱(Load-Balancing)에 대해서 알아보고자 한다.   

로드밸런싱(Load-Balancing)이란, 접속량이 많은 경우 서버의 부하를 줄이기 위해서 도입된 개념이다.   
즉, 두개 이상의 컴퓨터자원(서버)에 작업을 다누는 것을 의미한다.   

이때, OSI 7계층을 기준으로 로드밸런서 위치에 따라서 종류가 나뉘게 된다.   
L4(TransPort) Load-Balancing : 클라이언트의 IP 포트, TCP/UDP 세션 기준으로 분산.   
L7(Application) Load-Balancing : URL 패턴, 쿠키 등의 정보 기준으로 분산.   

---   

Tomcat의 Core는 Catalina로 되어있으며, 파일 구조는 아래와 같다.   
![Image](https://github.com/user-attachments/assets/c3865e82-a934-4eb8-b94c-1bdac4d8d87c)   

> bin : Tomcat 실행에 필요한 실행파일 및 스크립트 파일이 위치한다. (중요)   
> conf : 서버의 설정과 관련된 설정 파일들이 위치한다. (중요)   
> lib : Tomcat 구동 시 필요한 라이브러리와 WebServer와 Tomcat을 연결해주는 모듈이 포함되어있다.   
> logs : Tomcat 실행 관련 로그파일이 위치한다.   
> temp : 실행 시, 임시파일이 위치한다.   
> webapps : 웹어플리케이션이 위치된다.   
> work : jsp파일을 서블릿 형태로 변환한 java파일과 class파일.   

conf/server.xml 상에 서버 설정을 진행하고, conf/web.xml은 서버 동작 시 읽는 파일이다.   

운영에서 동작 시에는 webapps에 웹 어플리케이션을 배포하지 않고 보안을 위해 webapps 지우고 다른 폴더를 만들어 배포한다.  
추가로, server.xml 설정은 변경해줘야한다.   

또한, properties파일의 경우 운영모드와 개발모드파일이 다른 경우에는 bin/setenv.bat에 아래와 같이 설정을 해준다.   
```   
set "JAVA_OPTS=%JAVA_OPTS% -Dspring.profiles.active=dev"
```   

---

추가로, Tomcat구동 시에는 아래와 같이 명령어로 올리게 되는데 서비스로 등록하는 방법에 대해 설명한다.   

서비스 등록 명령어   

```   
service.bat install
tomcat톰캣 버전 //US//Tomcat톰캣버전 --Description="등록하고자 하는 서비스 명" ++JvmOptions -Dspring.profiles.active=dev
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions -Djdk.tls.ephemeralDHKeySize=2048
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions -Djava.protocol.handler.pkgs=org.apache.catalina.webresources
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions -DskipTests=true
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions --add-opens=java.base/java.lang=ALL-UNNAMED 
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions --add-opens=java.base/java.io=ALL-UNNAMED 
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions --add-opens=java.base/java.util=ALL-UNNAMED 
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions --add-opens=java.base/java.util.concurrent=ALL-UNNAMED 
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions --add-opens=java.rmi/sun.rmi.transport=ALL-UNNAMED
tomcat톰캣 버전 //US//Tomcat톰캣버전 ++JvmOptions -Dlog4j2.debug=true

```   

서비스 삭제 명령어   

```
service.bat remove
```   


위와 같이 등록하게 되면, 서비스목록에서 확인할 수 있다.   




--- 