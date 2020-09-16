---
title:  "[JSP]안드로이드스튜디오-JSP-DB연동 로그인프로그램(3)"
search: false
categories:
  - review
  - ERROR
tags:
  - review
  - Android
  - JSP
toc: true
toc_sticky: true
comments: true
---

저번시간부터 진행하고 있던 로그인프로그램 정리해요:)  
**혹시, 사진이 너무 안보인다면 150%로...해서 봐주세요:)**  
**번거롭겠지만 보면 꼭 도움될겁니다:)**  
**-jaejae올림-**  

## 1.JSP와 Android코드 분석하기  

오늘은 다음 4가지의 코드를 알아봐요.   


- 우선은 안드로이드 화면에서 버튼을 누르면 text박스의 데이터를 가져와요:)  
- 그 다음 CustomTask에 인자값을 넘겨줘서 처리를 요청합니다.
![Image Alt hello](/assets/img/20-09-15-p2.JPG)  


- 위에서 요청한 CustomTask를 알아봐요.  
- 저희가 만들어놨던 jsp파일을 요청하고, 거기에 sendMSG를 이용해서 보내줘요:)  
- **여기서 중요한 것은, 보이시는 ""안에 있는 변수명은 반드시 기억해주세요.**  
![Image Alt hello](/assets/img/20-09-15-p1.JPG)  


- 위에서 ""안의 변수를 기억하시나요?ㅎㅎ  
- **이곳에서 request.getParameter()안에 들어가는 변수와 반드시 일치해야합니다.**  
- 그 다음, 밑에 함수처럼 일정 파라미터 값이 나오면 LoginDB()라는 함수를 실행합니다.  
![Image Alt hello](/assets/img/20-09-15-p3.JPG)  


- 함수를 알아봐요:)  
- LoginDB()안의 인자들은 안드로이드측에서 받아서 JSP단에서 변수로 받은 값들입니다:)  
- 받은 값들이 데이터베이스 안의 값과 일치하면 "true"라는 값을 returns에 리턴하는 내용의 함수입니다:)  
![Image Alt hello](/assets/img/20-09-15-p4.JPG)  

**여기까지 이해가 되셨다면, 우리는 이제 연동을 정복한거랍니다:)  
jaejae의 리뷰 완료:b**  


## 2.전체 프로젝트 구조  
**< 안드로이드 프로젝트 구조 >**  
![Image Alt hello](/assets/img/20-09-14-p3.JPG)  

**< jsp 프로젝트 구조 >**  
![Image Alt hello](/assets/img/20-09-10-p4.JPG)  


## 3.전체 소스코드 공개  
**< jsp 코드>**  
<script src="https://gist.github.com/jaejae0015/c34f099f9d70bbb416408633a70cb3f9.js"></script>
<script src="https://gist.github.com/jaejae0015/86b6fc25956dca5b830e55615b9017d1.js"></script>
**< 안드로이드 코드>**  
<script src="https://gist.github.com/jaejae0015/c9e81a25855810ba1d5adf69069e3e6e.js"></script>
<script src="https://gist.github.com/jaejae0015/ef7a70324057ff50692f99d558c063a2.js"></script>
<script src="https://gist.github.com/jaejae0015/d7798a36ccbde28d61932ca88ae1057e.js"></script>
<script src="https://gist.github.com/jaejae0015/47cb8ab3176545ac5450e419b4a24c3c.js"></script>
<script src="https://gist.github.com/jaejae0015/448e5b3b49473db6c6f8e042c4aab9b2.js"></script>

**< 결과화면>**  
![Image Alt hello](/assets/img/20-09-14-p2.JPG)  

이상으로 안드로이드스튜디오-JSP-DB연동 로그인프로그램 리뷰를 마치겠습니다:)  

혹시, 질문사항이나 안되는 부분이 있으시다면 댓글 남겨주세요~  