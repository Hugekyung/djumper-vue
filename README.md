# djumper_vue
본 프로젝트는 E-commerce 웹 서비스 Djumper의 Front 개발 프로젝트입니다.<br> 
--> [Backend 이동](https://github.com/Hugekyung/djumper-django)<br><br/>
### Overview
---
프로젝트의 목적은 백엔드에서 구현한 API를 프론트엔드에서 어떻게 활용하는지, 백-프론트 사이의 통신 방법과 비동기 처리 방식을 비롯한 전체 웹 서비스 구축 프로세스를 경험하기 위함입니다.<br><br/>

#### Environment
```
OS                            Version
----------------------------- ---------
Ubuntu                        20.04 LTS(WSL)

Package                       Version  
----------------------------- ---------
              [Back-end]
Django                        3.2.6
django-cors-headers           3.8.0                
djangorestframework   
Pillow       
djoser

              [Front-end]
vue(with cli)
axios
bulma
```
<br><br/>

### Usage(Front)
---
```
// 1. CURL 설치
$ sudo apt install curl

// 2. PPA 추가(ubuntu에서 기본 제공하는 npm 패키지의 버전은 최신버전보다 다소 낮기 때문에 최신버전을 받기 위해 수행한다)
$ curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -

// 3. Node.js 설치(npm 설치 목적)
$ sudo apt install -y nodejs

// 4. vue & vue CLI 설치
$ sudo npm install -g @vue/cli
```
<br><br/>

### Project Result
---
images
