## 쿠팡 상품 리뷰 크롤링 (lxml + selenium)
쿠팡 페이지 크롤링도 마찬가지로 lxml로 url을 수집한 후 상품리뷰는 selenium을 통해 구현합니다. 하지만 selenium으로 구현할 때 단순히 chrome 페이지를 열면, 404 error를 마주합니다. 이를 subprocess 모듈을 통한 chrome.exe로 극복한 것이 본 페이지의 포인트입니다.


### 수집과정
1. url수집
2. 세부정보수집

### 1. url수집
 - lxml로 수집이 이루어집니다. 키워드를 검색한 후 판매량순으로 정렬시키고 판매량 상위 10개 품목의 url을 가져오고자 했습니다. 추가적으로 id와 상품명을 수집하였습니다.
 - id가 무엇일까요? 쿠팡의 세부정보 url은 다음과 같습니다. 
 
 https://www.coupang.com/vp/products/220853174?itemId=688970933&vendorItemId=4766900974&pickType=COU_PICK
 
  > 여기서 products/ 와 ?itemId 사이에 있는 것이 id입니다. 이를 추출한 이유는 쿠팡의 경우 같은 품목이지만 색깔이 다른 경우 다른 페이지에 등록하는 경우가 있습니다. 이들은 하지만 같은 리뷰를 공유하고 있습니다. 이러한 예시는 아래 url을 보시면 됩니다. 

          https://www.coupang.com/vp/products/95556823?itemId=294791268&vendorItemId=70953277151&isAddedCart=
          https://www.coupang.com/vp/products/95556823?itemId=294791270&vendorItemId=70953277133&isAddedCart=
          https://www.coupang.com/vp/products/95556823?itemId=294791265&vendorItemId=70953277117&isAddedCart=

  > 이들은 같은 리뷰를 공유하고 있기에, 정보를 2번 수집한다면 시간낭비입니다. 우리는 url을 1차적으로 수집한 후, id를 기준으로 중복제거하여 수집시간을 효율적으로 사용할 수 있습니다.

### 2. 세부정보수집
 - 세부정보 수집은 일반적인 selenium으로 크롤링할 경우 아래와 같은 문구를 마주합니다. 페이지 접속은 잘 되나, 리뷰가 있는 페이지에 접근하기 위해 click이벤트가 발생할 경우 쿠팡에서는 접근을 바로 차단합니다.

![image](https://user-images.githubusercontent.com/28617435/122664125-55f30e00-d1da-11eb-8cf2-41f781097890.png)
 
 - 우리는 이를 chrome.exe를 통해 극복할 수 있습니다. (이는 window만 해당사항이며, 기타 mac 등에는 적용이 안됩니다.) 아래 코드를 참조하시기 바랍니다.
 
![image](https://user-images.githubusercontent.com/28617435/122664166-9bafd680-d1da-11eb-9f8f-3d2f86bf3498.png)
 
 - 이후 <필수 표기정보>까지 스크롤을 내립니다. 상품평을 클릭하기에 가장 근처에 있는 요소가 <필수 표기정보>이기에 이를 선택했습니다. <필수 표기정보>에서 높이를 200정도 빼어 상품평이 화면에 보이도록 설정하고 상품평을 클릭합니다.

![image](https://user-images.githubusercontent.com/28617435/122664280-82f3f080-d1db-11eb-8669-068f2d7e8d87.png)


 - 완료되었다면 모든 별점보기까지 요소를 찾아 스크롤을 이동합니다. 하지만, 상품평이 없을경우 해당 요소가 존재하지 않기에, 예외처리를 실시하였습니다.

![image](https://user-images.githubusercontent.com/28617435/122664290-8a1afe80-d1db-11eb-84e4-20eeb927388e.png)

 - 이제 각 점수별 리뷰로 이동합니다. 나쁨, 보통, 별로에 대해 수집할 것이기에 이들을 각각 클릭합니다. 만약 아래와 같이 “별로”에서 리뷰가 없더라도 클릭이 가능하며, 오류가 발생하지 않기에 예외처리는 실시하지 않았습니다.

![image](https://user-images.githubusercontent.com/28617435/122664270-72dc1100-d1db-11eb-8235-c2530f884965.png)


 - 이제 각 상품에 대한 리뷰를 수집합니다. 수집하고자 하는 정보는 “글쓴이”, “별점”, “날짜”, “상품명”, “제목”, “내용”, “url”, “판매량순위” 이며 아래 5가지 항목 이외에 url은 해당 리뷰를 수집한 url, 상품명은 url을 가장열면 나오는 제품명입니다. 더불어 판매량 순위는 url수집 당시, 판매량순으로 정렬한 후 실시하였기 때문에 url_idx를 통해 도출할 수 있습니다.

 ![image](https://user-images.githubusercontent.com/28617435/122664261-5f30aa80-d1db-11eb-9703-0c0afd2b356a.png)


 - 여기서 주의할 점은 제목과 본문은 없는 경우가 있다는 것이죠. 없을 경우 오류를 회피하기 위해 예외처리를 실시하였습니다. 또한 리뷰가 많을 경우 페이지를 넘겨가며 수집할 수 있도록 구현하였습니다.


### 최종수집결과 예시

![image](https://user-images.githubusercontent.com/28617435/122664246-458f6300-d1db-11eb-8e34-53162b5a28f6.png)
