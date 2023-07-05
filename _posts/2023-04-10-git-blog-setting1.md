---
title: 한 대의 컴퓨터에서 Git 계정 두 개 사용하기
author:
  name: jaejae0015
  link: https://github.com/jaejae0015
date: 2023-04-10
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

회사에서 Git을 사용하는 경우, 개인 Git계정에 연결은 가능하나 커밋 시 회사 계정으로 커밋이 된다.

때문에, 한 대의 컴퓨터에서 2개 이상의 Git계정 연결 관련하여 작성을 해보고자 한다.

## 기본 준비 할 것
> OS : Window 10
> 
> <a href="https://git-scm.com/downloads">git 설치</a>-Standalone Installer
> 
> 개인용 GitHub 계정 생성해두기
>

---

## 2개 계정 연결하기
지금부터 아래 git 계정을 기준으로 진행합니다.
> GIT 계정 : userA / GIT 이메일 : userA@example.com
> 
아래의 명령어는 모두 설치된 **git Bash**에서 진행합니다.

---
1.SSH 디렉토리 생성 및 이동

  ```console
  #.ssh 디렉토리가 없는 경우 아래 명령어로 생성합니다.
  mkdir .ssh

  cd ~/.ssh
  ```
2.SSH-KEY 생성

```console
ssh-keygen -t rsa -C "userA@example.com" -f "id_rsa_userA"

암호 관련된 메시지가 나오면 Enter로 넘어갑니다.
``` 
3.SSH-KEY 생성 확인

```console
lS -al
``` 
4.SSH-Agent에 생성한 SSH-KEY 추가

  ```console
  #SSH Agent 백그라운드 실행
  eval "$(ssh-agent -s)"

  #SSH Agent에 키 등록
  ssh-add ~/.ssh/id_rsa_userA

  #키 등록 확인
  ssh-add -l
  ```  
5.생성한 SSH-KEY 파일 복사
  ```console
  #생성된 키 파일 확인 후, copy하기
  vi ~/.ssh/id_rsa_userA.pub
  ```  
6.gitHub에 userA로 로그인
7.로그인 후에, 우측 상단 프로필 클릭 > Settings로 이동
![image](https://user-images.githubusercontent.com/56392513/230844158-1bbfd016-5018-4571-8415-156b8c01774b.png)

8.아래와 같이 SSH Key 등록으로 들어가서 복사한 Key를 붙여넣습니다.
   ![image](https://user-images.githubusercontent.com/56392513/230830501-6043dc2f-2865-4ab7-be25-8f4b1caba1da.png)
  ![image](https://user-images.githubusercontent.com/56392513/230830943-07b2a6d3-e563-426d-83b0-662638e439d2.png)

9.~/.ssh 폴더에서 config 파일 작성
```console
#.ssh 디렉토리로 이동
cd ~/.ssh
#config파일이 없으면 새로 생성
vi config
```
10.i를 눌러서 입력 모드로 전환 후 아래 내용 입력+입력후에는 :wq!
> 여기서 Host 설정이 중요합니다. github.com-git계정명으로 통일하는게 편합니다.
>

```console
#userA의 SSH 설정
Host github.com-userA
    HostName github.com
    User userA
    IdentityFile ~/.ssh/id_rsa_userA
```

11.SSH 연결 테스트
```console
ssh -T git@github.com-userA
```

---
>  git Blog 생성을 이어 가실 분은 <a href="https://jaejae0015.github.io/posts/git-blog-setting2/">다음 글</a>을 이어서 확인해주세요.
{: .prompt-info }

> 그냥 Repository 다운은 아래 내용까지 하면 완성!
{: .prompt-info }

12.소스 SSH 주소 가져오기
![image](https://user-images.githubusercontent.com/56392513/230831998-5a15b15b-8035-46eb-be31-c15959cda666.png)

13.소스 clone 받기
```console
ssh -T git@github.com-userA:userA/userA-Repository.git
```
14.git 계정 변경 
```console
git config user.name userA
git config user.email userA@example.com
```
---

회사에서 git계정을 사용하고 있어서, 개인 git계정을 사용하기 위해서는 위와 같은 과정을 거쳐야했다..😂

나는 한 대의 컴퓨터에서 2개 이상의 git계정을 사용하는 것이 어렵다는 것도 이번을 통해 처음 알게됐다.

내가 작성한 글을 통해서, 다른 사람들은 조금 수월하게 연결에 성공했으면 좋겠다😀


---

## 참고 사이트
<a href="https://usingu.co.kr/frontend/git/%ED%95%9C-%EC%BB%B4%ED%93%A8%ED%84%B0%EC%97%90%EC%84%9C-github-%EA%B3%84%EC%A0%95-%EC%97%AC%EB%9F%AC%EA%B0%9C-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0/" target="_blank" rel="noreferrer noopener">한 컴퓨터에서 github 계정 여러개 사용하기</a>

<a href="https://devlog.jwgo.kr/2019/04/17/ssh-keygen-and-ssh-agent/" target="_blank" rel="noreferrer noopener">SSH Keygen을 이용한 키 생성 방법과 ssh-agent에 대한 간단 설명</a>
