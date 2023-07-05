---
title: IIS URL Rewrite 설치 및 헤더 없애기
author:
  name: jaejae0015
  link: https://github.com/jaejae0015
date: 2023-04-14
categories: [Developer, Etc]
tags: [Etc]
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

서버에서 외부망이 안되는 경우, 웹 인스톨러 및 다른 다운로드 사이트 접근이 안된다.
그럴 경우, 아래 경우를 참고하여 헤더의 중요 정보를 삭제하도록 한다.

---

1.아래의 사이트에 접근하여, 각 서버 버전에 맞는 installer를 다운받는다.

<a href="https://iis-umbraco.azurewebsites.net/downloads/microsoft/url-rewrite">https://iis-umbraco.azurewebsites.net/downloads/microsoft/url-rewrite</a>
![image](https://user-images.githubusercontent.com/56392513/231332318-fa656ccd-96c7-4eea-8242-47c440495bfb.png)

2.설치 후, IIS에서 URL재작성이라는 메뉴가 뜬 것을 확인한다.

![image](https://user-images.githubusercontent.com/56392513/231332601-be1d77cb-5356-4110-85eb-05f5fa0678c4.png)

3.아래 사이트를 참고하여 변수 작성을 해준다. 

> <a href="https://gunnm.tistory.com/122"> https://gunnm.tistory.com/122 </a>
>

---

댓글에 오류사항이나, 잘못된 부분을 알려주시면 반영하겠습니다:)

---

