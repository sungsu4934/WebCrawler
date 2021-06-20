## 네이버 지도 크롤링입니다. (selenium)

아래 url을 기반으로 크롤링을 하였습니다. 행정동명 정보가 필요하기에 예시를 위해 세종특별자치시의 행정동 엑셀파일을 추가 첨부하였습니다. 대표적인 예시로 세종특별자치시 내 pc방, 노래방, 보드게임카페를 크롤링하고자 합니다.

https://m.map.naver.com/

### 수집과정

#### 0. 개요
 - 목록보기를 눌러야 정보를 얻을 수 있기에 selenium을 활용하여 click event를 발생시킵니다. 추가적으로 목록보기 후 스크롤을 내릴 때도 selenium을 활용해 구현할 수 있습니다.


#### 1. url 접근
 - 동적 크롤링이 필요한 페이지인 만큼 selenium을 활용하였습니다. 아래 url에 각각 시/도, 시/군, 키워드명을 대입해 url에 접근합니다. 접근 후, 목록보기를 누르고 스크롤을 가장 아래까지 내려 html내 모든 정보를 스캔한 후 저장될 수 있도록 합니다. 

  > 한 번 스크롤을 제일 아래로 내린다고 가장 마지막 항목이 보이는 것이 아닙니다. 스크롤을 내리면 일정 개수만큼만 업데이트 되며, 추가적으로 스크롤을 내려야 그 이하 정보를 수집할 수 있습니다.

       https://m.place.naver.com/place/list?query={시/도}%20{시/군}%20{키워드명}

![image](https://user-images.githubusercontent.com/28617435/122665938-4c22d800-d1e5-11eb-905d-3e2e7af0d2ed.png)

#### 2. 정보수집
 - 수집되는 정보는 ['id','가맹점명','naver_category','전화번호','lat','lng','도로명주소']입니다. 코드 내 value1 ~ value8의 path를 잘 따라가면 어떤 것을 나타내는지 알 수 있습니다.

#### 3. driver 초기화
 - 더불어 “세종특별자치시 조치원읍”처럼 시/군 단위가 끝나면 driver를 껐다가 킵니다. 데이터 수집이 많이 반복될 경우 chromedriver 내부적으로 수집이 느려져 초기화 작업을 실시하기 위함입니다.

### 최종수집결과

![image](https://user-images.githubusercontent.com/28617435/122665681-96a35500-d1e3-11eb-97d2-b496a4e65335.png)


### 첨언
 - 아래 두 url이 달라 헷갈리시는 분들이 있으실 텐데, 실제로 검색해보신다면 동일한 결과를 얻으실 수 있습니다!

       https://m.map.naver.com/
       https://m.place.naver.com/place/list?query={시/도}%20{시/군}%20{키워드명}
      
 - url 예시

       https://m.place.naver.com/place/list?query=세종특별자치시%20조치원읍%20pc방
