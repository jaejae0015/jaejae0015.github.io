---
title: git 블로그 시작하기 - Chirpy + 댓글(Utterances)기능 추가
author:
  name: jaejae0015
  link: https://github.com/jaejae0015
date: 2023-04-13
categories: [Developer, Git Blog]
tags: [Git Blog]
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

나의 게시물을 보고, 누군가는 환경이 달라 에러가 발생할 수 있다.

내가 발생했던 오류라면 해당 에러에 도움을 줄 수 있을 것이다!ㅎㅎ

에러까지 책임질 수 있는 게시물이 되기 위해 댓글 기능을 추가해보려고 한다😉

---

## 기본 준비 할 것
> OS : Window 10
1. 개인 git 계정과 git Blog

---
## Utterances 설치하기

대부분의 git Blog는 댓글기능을 지원하고 Disqus를 이용한다.

Chirpy의 경우, 따로 댓글기능을 따로 지원하지 않는 것 같다.

댓글 기능을 지원하는 프로그램 중, Git 계정이 있는 사용자만 작성가능 한 Utterances를 사용해보려고 한다.


1.GitHub에 로그인 후, 아래 표시된 바와 같이 MarketPlace > Utterances를 검색한다.

![image](https://user-images.githubusercontent.com/56392513/231032633-34b1e412-73d8-4f58-b93d-ad7941a03fa8.png)

2.검색된 Utterances를 눌러서, 하단의 install을 눌러준다.

![image](https://user-images.githubusercontent.com/56392513/231032859-6492dafe-eb6b-4b2d-a75e-bd2c7519bf6f.png)

3.install 후, 적용하려는 Repository를 선택한다.

![image](https://user-images.githubusercontent.com/56392513/231034216-5cdc6d6e-c214-4c36-9e3c-8eecdc13ad20.png)

3.선택 후, <a href="https://utteranc.es/">나오는 페이지</a>에서 아래 부분만 작성하여 하단의 코드를 복사합니다.
![image](https://user-images.githubusercontent.com/56392513/231034542-8483a6a3-7380-466f-a0e1-2c1c4f0891a2.png)

![image](https://user-images.githubusercontent.com/56392513/231034681-9e4b4c56-9600-4c26-9583-7c675735c635.png)

4.복사한 코드는 <kbd>_layouts/post.html</kbd>의 하단에 붙여줍니다. 
<a href="https://github.com/jaejae0015/jaejae0015.github.io/blob/main/_layouts/post.html">참고</a>

<script src="https://gist.github.com/jaejae0015/feb0dfcd16d69f97b18529e61a72dfad.js"></script>

5.push 하여 확인합니다.

```console
# localhost:4000에서 확인 후, push를 권장합니다.
jekyll serve

# 댓글 기능이 추가되었는지 확인 후, push
git add -A
git commit -m "댓글기능"
git push

```


---

위와 같이 진행하면, Chirpy Theme에서도 댓글기능을 사용할 수 있습니다😎😎

댓글에 오류사항이나, 잘못된 부분을 알려주시면 반영하겠습니다:)

---

## 참고 사이트

<a href="https://dev-yakuza.posstree.com/ko/jekyll/utterances/" target="_blank" rel="noreferrer noopener">
Jekyll 블로그에 댓글 기능 넣기</a>

