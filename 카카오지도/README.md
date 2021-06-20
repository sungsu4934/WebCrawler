## API를 활용한 카카오 지도 크롤링입니다.
카카오 내부에서도 API 서비스를 제공합니다. 간단하게 API 사용법을 제시하며 예시로 위도, 경도 좌표를 넣었을 때, 도로명주소와 지번주소를 API를 통해 갖고오는 것을 보여드리고자 합니다.

## 1. 카카오 API Key 받아오기
 우선적으로 아래 사이트에서 key를 발급받아야 합니다. 카카오의 경우 1일 30만회까지는 무료로 제공하고있습니다. 절차가 잘 나와있으니 해당 과정을 밟아 Key를 받아옵니다. 우리에게 필요한 key는 REST API키 입니다.

https://apis.map.kakao.com/web/

![image](https://user-images.githubusercontent.com/28617435/122691301-c301b480-d269-11eb-93f4-477858796d7d.png)

## 2. Document 기반으로 나에게 필요한 API를 식별 및 요청 파라미터 파악

- 저는 좌표를 이용하여 주소를 뽑아내는 과정이 필요하기에, 아래 사이트에서 제시한 API방법론을 사용하였습니다.

  https://developers.kakao.com/docs/latest/ko/local/dev-guide#coord-to-address

 - 꼭 들어가야하는 위도 및 경도를 위치시켰고, 아래 제시한 것과 같이 REST_API KEY를 위치시켰습니다.

![image](https://user-images.githubusercontent.com/28617435/122691368-3f949300-d26a-11eb-84bb-4da8062ab1eb.png)

 - 아래는 제 코드입니다. 간단하게 python 내 request 모듈만으로 이를 가져올 수 있습니다.

![image](https://user-images.githubusercontent.com/28617435/122691387-5f2bbb80-d26a-11eb-8e3c-2b613339728a.png)


## 3. 정보수집
 - 저는 세종특별자치시 내 일부 위도, 경도를 샘플로 10개 가져와 진행하였습니다.
  
![image](https://user-images.githubusercontent.com/28617435/122691461-c184bc00-d26a-11eb-8e4a-f50dba9c9c67.png)

 - 이제 해당 자료를 API에서 제시한 올바른 형태로 넣어줍니다. 결과는 아래와 같습니다! 실제로 도로명주소 또는 지번주소가 존재하지 않는 것들은 공백으로 처리하였습니다!
 
![image](https://user-images.githubusercontent.com/28617435/122691483-eaa54c80-d26a-11eb-9d68-395f10b16b21.png)
