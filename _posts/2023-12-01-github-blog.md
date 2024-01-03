---
title: '[Github Blog] Jekyll Chirpy 테마로 블로그 커스터마이징 하기 (Mac)'
date: 2023-12-01 +0900
categories: [github, blog]
tags: [blog, github, git, jekyll, chirpy, theme, customizing, custom, mac, macbook]
pin: true
img_path: /posts/2023-12-01/
---

jekyll chirpy theme 소스코드를 직접 다운 받아 블로그 커스터마이징 하기

## 1. Github Repository 만들기

### 1-1. Github 계정 생성하기

[**github**](https://github.com) 계정이 없다면 새로 만들자.

### 1-2. 블로그용 new repository 생성하기

블로그용 레포는 다른 레포와는 달리 계정당 1개만 만들 수 있다.  
레포지토리 이름이 중요한데, `(username).github.io`로 만들어야 한다.  
README.md는 나중에 충돌 문제가 생길 수 있으니 체크하지 말고 생성하자.  
![Desktop View](img1.png){: w="600" h="300"}
_나는 이미 만들어서 오류가 나는 것이니 무시해도 된다_ 

<br>
## 2. Jekyll 설치 및 실행하기

### 2-1. Jekyll 설치하기

- mac은 `rbenv`를 이용하여 `jekyll`을 설치해야한다.
```zsh
$ brew install rbenv ruby-build
```

- 설치 가능한 버전 중 3 버전 이상의 최신 버전을 설치한다.
```zsh
$ rbenv install -l
$ rbenv install 3.1.4 
```

- ruby 버전을 확인하고 변경해준다.
```zsh
$ rbenv versions
$ rbenv global 3.1.4
$ rbenv versions
```

- ruby PATH를 설정해준다. 나는 zsh shell을 사용하고 있기 때문에 `~/.zshrc` 파일에 추가했다.
```zsh
$ echo 'if which rbenv > /dev/null; then eval "$(rbenv init -)"; fi' >> ~/.zshrc
$ source ~/.zshrc
```

- `jekyll`을 설치해준다.
```zsh
$ gem install jekyll bundle
```   

### 2-2. nvm 설치하기

- 정상적인 배포를 위해 설치해준다.
```zsh
$ brew install nvm
$ mkdir ~/.nvm
$ echo 'export NVM_DIR=~/.nvm' >> ~/.zshrc
$ echo 'source $(brew --prefix nvm)/nvm.sh' >> ~/.zshrc
$ source ~/.zshrc
$ nvm install --lts
```

### 2-3. chirpy 테마 소스코드 다운받기

- [**jekyll-theme-chirpy**](https://github.com/cotes2020/jekyll-theme-chirpy)에 들어가서 `Code`옆의 화살표를 누르고 `Download ZIP`을 선택한다.
    ![Desktop View](img5.png)  

### 2-4. local repository 생성하기

- 다운 받은 소스코드를 원하는 위치로 변경한다.
- 폴더 이름을 `(username).github.io`로 변경한다.
- **[중요]** 터미널 경로를 다운받은 폴더로 변경한 뒤, 로컬 설정을 진행한다.
```zsh
$ cd ~/rudeuns.github.io
$ git init
$ bundle
$ npm install && npm run build
```

- 모든 설치가 완료됐다면 로컬에서 실행시켜본다.
```zsh
$ jekyll serve
```

- 웹 브라우저에 <http://127.0.0.1:4000/> 주소를 입력 후 정상적으로 실행되는지 확인한다.
- 터미널에서 `ctrl + c`를 입력하면 종료된다.

<br>
## 3. 빌드 및 배포하기

### 3-1. 로컬에서 기존 workflows 삭제하기

- `.github`의 모든 파일 및 디렉토리를 삭제한다.
- 만약 보이지 않는다면 mac의 finder에서 `command + shift + .`을 입력한다.
- `.github` 디렉토리에 `workflows` 디렉토리를 다시 생성한다. (이 디렉토리를 삭제하지 않고 내부 파일들만 삭제했어도 된다.)

### 3-2. 새로운 workflows 생성하기

- github repository에서 `Settings -> Pages`로 들어간다.  
- `Source`를 `Github Actions`로 변경한다.
- 아래의 workflows 중 `Jekyll`의 `cofigure`를 클릭한다.
    ![Desktop View](img2.png)

- 오른쪽 상단의 `Commit changes...`를 클릭한다.
    ![Desktop View](img3.png)

- 수정없이 `Commit changes`를 클릭한다.
    ![Desktop View](img4.png){: w="500" h="250" .normal}

- `Settings -> Branches -> Add rule`을 클릭한다.
- `Branch name pattern`을 `main`으로 입력하고, 전부 체크 해제된 상태로 `Create`를 클릭한다.
    ![Desktop View](img6.png)

- github repository 주소를 복사한다.
    ![Desktop View](img7.png)

- github에서 생성한 workflows를 가져오기 위해 원격과 로컬 레포를 연결시킨다.
```zsh
$ git remote add origin (자신의 원격 레포 주소)
$ git pull origin main
```

### 3-3. gitignore 수정하기

- **[중요]** `.gitignore` 파일에서 `assets/js/dist` 부분을 주석처리한다.

### 3-4. 원격으로 push 하기

- 배포한다.
```zsh
$ git add -A
$ git commit -m "Initial commit"
$ git push -u origin main
```

- github repository에서 `Actions` 탭을 클릭하면 배포 상황을 확인할 수 있다.
- 처음 생성했던 workflow는 실패라고 뜨는데 무시해도 된다.
- 방금 push한 commit이 정상적으로 배포되었다면 웹브라우저 주소창에 `(username).github.io`를 입력해서 확인해보자.
    ![Desktop View](img8.png)

- 로컬 레포에서 새로운 포스트 생성 및 수정 시 터미널에서 `jekyll serve` 실행 후 웹 브라우저에서 확인한다.
- 수정이 완료됐다면 `commit -> push`하여 배포하면 된다.
- post 작성 방법은 `_posts` 폴더의 파일들을 참고한다. 필요없다면 삭제해도 된다.
