# djumper_vue
본 프로젝트는 E-commerce 웹 서비스 Djumper의 Front 개발 프로젝트입니다.<br> 
--> [Backend 이동](https://github.com/Hugekyung/djumper-django)<br><br/>
## `Overview`
프로젝트의 목적은 백엔드에서 구현한 API를 프론트엔드에서 어떻게 활용하는지, 백-프론트 사이의 통신 방법과 비동기 처리 방식을 비롯한 전체 웹 서비스 구축 프로세스를 경험하기 위함입니다.<br><br/>

### Environment
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

## `Usage(Front)`

### 1. CURL 설치
```
sudo apt install curl
```
<br>

### 2. PPA 추가
- ubuntu에서 기본 제공하는 npm 패키지의 버전은 최신버전보다 다소 낮기 때문에 최신버전을 받기 위해 수행한다.
```
curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -
```
<br>

### 3. Node.js 설치(npm 설치 목적)
```
sudo apt install -y nodejs
```
<br>

### 4. vue & vue CLI 설치
```
sudo npm install -g @vue/cli
```
<br><br/>

## `Project Images`
### 메인 페이지
![dv-main](https://user-images.githubusercontent.com/67989121/132650461-b2e15e16-cbcb-4880-b687-1e04bad59a35.PNG)
<br>

### 상품 리스트
![dv-list](https://user-images.githubusercontent.com/67989121/132650564-dd672845-9942-4f58-b063-5b10228f06be.PNG)
<br>

### 상품 검색
![dv-search](https://user-images.githubusercontent.com/67989121/132650659-54f20076-6b13-4140-a38c-1f38267fcba7.PNG)
<br>

### 상품 장바구니
![dv-shopping](https://user-images.githubusercontent.com/67989121/132650724-531d3e89-d3db-4c9f-a5b7-b629312301ca.PNG)
<br>

### 결제 페이지
![dv-payment](https://user-images.githubusercontent.com/67989121/132650778-f6d74805-9591-4c3a-bf1b-300dac84245c.PNG)