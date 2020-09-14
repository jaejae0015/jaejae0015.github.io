---
title:  "[JSP]안드로이드스튜디오-JSP-DB연동 로그인프로그램(2)"
search: false
categories:
  - review
  - ERROR
tags:
  - review
  - Android
toc: true
toc_sticky: true
comments: true
---

저번시간부터 진행하고 있던 로그인프로그램을 만들어봐요~  
**오늘은 Android-JSP 연동을 진행하겠습니다:)**  

## 1.Android 기본세팅하기.  
(단, Android Studio와 AVD가 세팅되어있다는 점을 전제로 함.)  

- 우선, Android Studio를 켜주시고, 프로젝트를 생성해주세요.  
![Image Alt hello](/assets/img/20-09-14-p1.JPG)  

- 그 다음, AndroidManifest.xml 파일을 열어주세요.  
AndroidManifest.xml 파일은 어플의 설정요소들을 명세해 놓는 파일입니다.  
이곳에서 어플이 웹서버에 연결할 수 있도록 인터넷 권한을 주겠습니다.  
다음의 **4~6번 코드와 16번 코드**를 참고하여 진행해주세요.  
<script src="https://gist.github.com/jaejae0015/c9e81a25855810ba1d5adf69069e3e6e.js"></script>


이렇게 되면 기본 세팅 완료:)  

## 2.Android 코드작성하기.  

우선은, 어떤형식으로 진행할 것인지 대략적으로 말씀드리고, 진행해볼게요:)  
저번시간에 넣었던 id와 pw를 넣고 로그인을 하면,  
DB데이터와 일치하게 되면 다음의 화면으로 넘어가는 프로젝트 구성으로 진행합니다.  

![Image Alt hello](/assets/img/20-09-14-p2.JPG)  

결과화면을 보셨나요? 그렇다면, 프로젝트 구조를 보고 코드공개합니다:)  

![Image Alt hello](/assets/img/20-09-14-p3.JPG)  


- xml파일  
<script src="https://gist.github.com/jaejae0015/ef7a70324057ff50692f99d558c063a2.js"></script>
<script src="https://gist.github.com/jaejae0015/d7798a36ccbde28d61932ca88ae1057e.js"></script>

- java파일  
<script src="https://gist.github.com/jaejae0015/47cb8ab3176545ac5450e419b4a24c3c.js"></script>

<script src="https://gist.github.com/jaejae0015/448e5b3b49473db6c6f8e042c4aab9b2.js"></script>


그 다음, 톰캣을 실행해주고 다음의 화면 결과가 나오면 완성.  

![Image Alt hello](/assets/img/20-09-14-p2.JPG)  


## 3.ERROR을 해결해봐요  

**- java.io.IOException: Cleartext HTTP traffic to 본인아이피 not permitted**  
   해당 에러는 AndroidManifest에 인터넷권한을 안줬을떄입니다.:)  
   반드시 권한을 넣어주세요:b  

   <script src="https://gist.github.com/jaejae0015/c9e81a25855810ba1d5adf69069e3e6e.js"></script>


이상으로 Android-JSP 연동에 대한 리뷰를 마치겠습니다:)  
다음시간에는, 코드설명을 하겠습니다~

혹시, 질문사항이나 안되는 부분이 있으시다면 댓글 남겨주세요~  