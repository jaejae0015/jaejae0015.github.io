---
title:  "[JSP]안드로이드스튜디오-JSP-DB연동 로그인프로그램(1)"
search: false
categories: 
  - review
  - ERROR
tags:
  - review
  - JSP
  
toc: true
toc_sticky: true
comments: true
---

오늘부터 앞으로 3단계에 거쳐서,  
안드로이드-JSP-DB연동을 해보고자 합니다:)  

시작하기에 앞서, 간략하게 '왜 굳이 JSP를 이용해서 연동을 진행해야하는가?'에  
대하여 설명하고 진행해볼까요?  

간단한 DB연동 프로그램을 할 때는, 보안적인 측면을 고려할 필요가 없습니다.  
하지만, 조금 규모 있는 프로젝트를 진행할 때는 직접적으로 데이터를 넣게 되면  
**보안상의 문제가 발생할 수 있어 웹서버를 통해서 DB와 통신**을 하게 됩니다.  

간략하게 구조를 설명하면 다음과 같습니다:)  
![Image Alt hello](/assets/img/20-09-10-p1.JPG)


**오늘은 일단 jsp-db연동을 진행하겠습니다:)**  
저는 위에서 말씀드린바와 같이 이클립스로 jsp를 구현하고 오라클로 db를 사용했습니다.  

## 1.오라클 DB에 테이블 생성하기  

- project라는 테이블을 생성해주세요.  

```sql
	CREATE TABLE project (id VARCHAR2(20), pwd VARCHAR2(20));
```


- 데이터를 한개정도 삽입할게요.  

```sql
	INSERT INTO project values ('jaejae', '12341234');
```

- 최종적으로 테이블 생성과 데이터 삽입이 잘 되었는지 확인할게요.  

```sql
	SELECT * FROM project;
```

다음과 같이 결과가 출력되면 DB는 완성되었습니다:)  

![Image Alt hello](/assets/img/20-09-10-p2.JPG)  


## 2.JSP에서 프로젝트 생성하기 및 DB연동 기본 작업.  

자, 지금부터 JSP를 이용하여 DB와 연동을 할 것입니다.  
시작하기에 앞서  
**1.간략한 프로젝트 준비**와 **2.OJDBC드라이버**를 가져올게요:)  
**(단, tomcat서버가 이미 설치 되어 이클립스에 있다는 것을 전제로합니다.)**  

**- Eclipse EE를 켜주세요**  
![Image Alt hello](/assets/img/20-09-10-p3.JPG)  

**우선은 제가 만든 Dynamic WebPorject 폴더를 보여드리겠습니다.  
기본 지식이 있으신 분들은 참고가 되기를 바랍니다:b**
![Image Alt hello](/assets/img/20-09-10-p4.JPG)  

위와 같이, 내용은 아직 채우시지 마시고 두가지의 파일을 생성할게요.  
**1)`/WebCcntent`밑에 login.jsp 파일을 만들어주세요.**  
![Image Alt hello](/assets/img/20-09-10-p6.JPG)  

**2)`/src`밑에 login.java 파일을 만들어주세요.**  
(저는 /src밑에 DB라는 패키지를 만들고 파일을 생성했어요)  
![Image Alt hello](/assets/img/20-09-10-p7.JPG)  

**3)`/WebCcntent/WEB-INF/lib`밑에 ojdbc 파일을 넣어주세요.**  

**- 오라클을 설치하신 위치로 가주세요.**  
  `C:/app/사용자명/product/11.2/dbhome_1/jdbc/lib`  
![Image Alt hello](/assets/img/20-09-10-p5.JPG)  

**- 위의 파일을 복사해서 `/WebCcntent/WEB-INF/lib`밑에 넣어주세요**  
![Image Alt hello](/assets/img/20-09-10-p4.JPG)  

이제 기본 세팅 끝났어요:)  

## 3.JSP프로젝트 소스코드 작성.  

자, 이제 어떤 방식으로 연동을 확인할지에 대해서 간략하게 말씀드리고  
소스코드 공개합니다:)  
아까, project라는 테이블에 데이터 하나를 넣은 것 기억하시나요~?  
해당 데이터가 존재하면 **"jsp db연동 성공"**이라는 문구를 jsp페이지(chrome)에 띄울 것입니다.  
또한 데이터가 있다는 것을 JSP가 알게되는 것이라 연동까지 확인이 된다는 점~  

jsp에서 동작하는 함수를 <%%>를 이용하여 작성할 수 도 있지만,  
소스 내용들이 길어지기 때문에 `/src`밑에 java파일로 작성할게요.  
**[Java에 있는 LoginDB()함수는 일단 이해가 안되더라도 우선 넣어주세요:)   
추후 설명합니다]**  

- `/WebContent/login.jsp` 파일입니다.  
<script src="https://gist.github.com/jaejae0015/c34f099f9d70bbb416408633a70cb3f9.js"></script>

- `/src/Login.java` 파일입니다.  
<script src="https://gist.github.com/jaejae0015/86b6fc25956dca5b830e55615b9017d1.js"></script>

다 넣으셨나요?  이제 RUN을 해서 확인할게요.  
![Image Alt hello](/assets/img/20-09-10-p8.JPG)  
위와 같은 화면이 나오면 완료~  

## 4.발생할수 있는 ERROR를 정리해봐요.  
**- No suitable driver found for jdbc**  
   해당 에러는 OJDBC가 없을 경우 발생하는 에러입니다:)  
   위쪽 2번의 기본 작업을 참고해서 OJDBC를 넣어주세요.

   - 오라클을 설치하신 위치로 가주세요 `C:/app/사용자명/product/11.2/dbhome_1/jdbc/lib`  
   ![Image Alt hello](/assets/img/20-09-10-p5.JPG)  

   - 위의 파일을 복사해서 `/WebCcntent/WEB-INF/lib`밑에 넣어주세요.  
   ![Image Alt hello](/assets/img/20-09-10-p4.JPG)  


**- HTTP 404에러**  

   ![Image Alt hello](/assets/img/20-09-10-p9.JPG)  
   
   해당 에러는 .jsp파일의 위치가 `/WebContent`밑에 위치하는 것이 아니라  
   `/WebContent/Web-INF`밑에 위치할 때 발생하는 에러입니다.  
   위치를 다시 확인해주세요.  
   
   ![Image Alt hello](/assets/img/20-09-10-p4.JPG)  

**- java.sql.SQLException: Access denied for user**  
  `/src/login.java`의 jdbcurl, id, pwd가 본인의 컴퓨터와 맞는지 확인해주세요.  
  ![Image Alt hello](/assets/img/20-09-10-p10.JPG)  


이상으로 JSP-DB연동에 대한 리뷰를 마치겠습니다:)  


혹시, 질문사항이나 안되는 부분이 있으시다면 댓글 남겨주세요~  