---
title: git 블로그 시작하기 - Chirpy theme 사용
author:
  name: jaejae0015
  link: https://github.com/jaejae0015
date: 2023-04-11
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

git 계정을 2개 사용하기 위해 세팅한 내용들을 잘 정리해두고 싶어서 개발 블로그를 시작하려했고, 

GIT 블로그의 Chirpy 테마 적용을 위해 힘들게 노력한 나의 방법들을 공유하려고 한다.

이 글을 보고 따라하는 분들은 쉽게 Chirpy 테마를 적용했으면 좋겠다😂   

---

## 기본 준비 할 것   
> OS : Window 10   
1. <a href="https://git-scm.com/downloads">git 설치</a>-Standalone Installer   
2. 개인용 GitHub 계정 생성해두기   
3. <a href="https://rubyinstaller.org/downloads/">Ruby 설치</a>-with DevKit   

---
## Git Hub Repository 생성 및 Clone 하기   
1.GitHub에 접속하여, <kbd>gitHub계정명.github.io</kbd>으로 Repository를 생성해준다.   
2.생성한 Repository를 Clone하기
  > 사용하는 컴퓨터에서 GIT 계정이 2개 이상인 경우
  >   

  ```console
  git clone git@github.com-userA:userA/userA.github.io.git

  git config user.name 사용할 git계정명
  git config user.email git계정 이메일
  ```   
  > 그냥 일반적인 Clone   
  >   

  ```console
  git clone https://github.com/userA/userA.github.io.git
  ```
---

## Jekyll Theme 가져오기-Chirpy

Git Blog의 테마는 대부분 <a href="http://jekyllthemes.org/">http://jekyllthemes.org/</a> 에서 확인 후,
해당 테마의 Git에 접속하여 fork 또는 다운받는다.

나는 해당 사이트에서 Chirpy를 선택했고, 해당 적용방법을 설명하려고 한다.

1.아래 Chirpy 테마 주소에 접속한다.

Chirpy Git 주소 : <a href="https://github.com/cotes2020/jekyll-theme-chirpy/">https://github.com/cotes2020/jekyll-theme-chirpy/</a>

2.접속 후, 아래와 같이 Release에 들어간다.  

![image](https://user-images.githubusercontent.com/56392513/230857560-1a314994-2735-4310-9a1c-84966142c635.png)

가장 최신 버전이 아닌, 아래 버전으로 다운받는다.  

![image](https://user-images.githubusercontent.com/56392513/230857825-2eb240b6-0a30-45db-9b76-f84889a4c57c.png)

3.다운받은 파일을 압축 해제하여, Clone한 폴더에 붙여넣는다.  

---

## Chirpy 테마 세팅해주기

Chirpy테마 적용 시, --- layout: home # index page --- 에러가....아주 많이 발생한다.

Mac에서는 해당 테마가 편리하게 세팅 가능하며, 구글링을 통해 정답을 쉽게 찾을 수 있다.

다만, 나는 Window 환경에서 Chirpy 테마를 적용하다보니 여러 에러가 발생하였고 아래와 같이 해결하였다.

1.본인의 .github.io 레포지토리 > Settings> Actions에서 아래와 같이 설정을 해준다.
![image](https://user-images.githubusercontent.com/56392513/230860441-dfdd5c08-fb70-4c46-b41b-e4df74541770.png)

2.아래의 파일들을 확인하여 삭제 및 세팅해준다.
> Gemfile.lock 삭제  
> .travis 삭제  
> _posts 아래의 .md 파일들 삭제  
> .github/workflows/pages-deploy.yml.hook 파일만 남기고 다 삭제  
> .github/workflows/pages-deploy.yml.hook 이름변경을 통해 .hook 지우기  

3..gitignore 파일이 아래와 같은지 확인   

```console
# hidden files
.*
!.git*
!.editorconfig
!.nojekyll
!.travis.yml

# bundler cache
_site
vendor
Gemfile.lock

# rubygem
*.gem

# npm dependencies
node_modules
package-lock.json
```

4..github/workflows/pages-deploy.yml 에서 ruby-version 변경
> 본인이 다운받은 ruby 버전으로 해준다. 나는 3.2.2로 변경하였다.   

```yml
name: 'Automatic build'
on:
  push:
    branches:
      - main
    paths-ignore:
      - .gitignore
      - README.md
      - LICENSE

jobs:
  continuous-delivery:

    runs-on: ubuntu-latest

    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          fetch-depth: 0  # for posts's lastmod

      - name: Setup Ruby
        uses: ruby/setup-ruby@v1
        with:
          ruby-version: 3.2.2
          bundler-cache: true

      - name: Deploy
        run: bash tools/deploy.sh

```

5._config.yml 에서 url 변경   

```console

# fill in the protocol & hostname for your site, e.g., 'https://username.github.io'
url: 'https://userA.github.io/'

```

6.ruby command 창 실행 후 아래 명령어 입력(관리자 권한으로 실행 권장)

```console
cd 본인이 clone 받은 위치

bundle install

jekyll serve

```

7.<kbd>localhost:4000</kbd>을 통해 사이트가 뜨는지 확인
8.확인 후, 정상적으로 출력이 되면 아래 명령어를 통해 본인 repository에 커밋

```console
#변경된 파일을 모두 스테이징.
git add -A

#메시지 작성 후 커밋
git commit -m "작성하고자 하는 메시지"

#commit 진행하기
git push

```

9.커밋 및 푸시 완료 후에 레포지토리의 Action탭에서 정상 build가 되는지 확인
![image](https://user-images.githubusercontent.com/56392513/230865137-538b4dd6-a184-413e-a394-ce67699c2071.png)

10.확인 후, Settings에 들어가서 아래와 같이 gh-pages 브랜치가 생겼는지 확인 후 변경할 것.

--- layout: home # index page --- 에러가 발생해도 아래 사항을 확인 할 것.
![image](https://user-images.githubusercontent.com/56392513/230865315-4ebcd844-7ab9-49b2-a9b5-565b9aab0b0e.png)

---

단순하게 git 블로그에 글을 남기고 싶었는데, 글 쓰는 것보다 테마적용이 더 오래걸렸다..😎

나의 글을 통해서 조금은 수월하게 Window에서도 적용했으면 좋겠다:)

---

## 참고 사이트
<a href="https://www.irgroup.org/posts/jekyll-chirpy/" target="_blank" rel="noreferrer noopener">Jekyll Chirpy 테마 사용하여 블로그 만들기</a>

<a href="https://seong6496.tistory.com/267" target="_blank" rel="noreferrer noopener">Github 블로그 테마적용하기(Chirpy)</a>

<a href="https://velog.io/@hashnsalt/Github-Blog-%EB%A7%8C%EB%93%A4%EA%B8%B0-2" target="_blank" rel="noreferrer noopener">Github Blog 만들기-2</a>
