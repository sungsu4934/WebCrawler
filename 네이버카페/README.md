## 네이버 카페 크롤링에 대해 다루고자 합니다. (selenium)
흔히 네이버 카페에 좋은 정보가 있어서 링크를 저장했다가 나중에 다시 들어가려고 하면 카페가입을 요구할 때가 있습니다. 이 경우 정말 곤란하죠. 타 페이지 크롤링에는 lxml, selenium, bs4를 섞어가면서 사용했으나 이번엔 selenium만으로 구현한 것을 보여드리고자 합니다.

![image](https://user-images.githubusercontent.com/28617435/122663090-06f5aa80-d1d3-11eb-828d-145de445ebf2.png)

본 파트에서는 이를 우회하는 저만의 노하우를 알려드리며, 네이버 카페 크롤링의 전체적인 과정을 살펴보고자 합니다.

### 수집과정
1. url 수집
2. 세부정보수집

### 1. url수집
 - 검색하고자하는 키워드를 입력하고 크게 어려운부분은 없습니다. [거래글제외]만 필터링하여 url에 옵션을 추가하고, q파라미터에 검색하고자하는 단어를 입력한 후 page파라미터만 바꿔가며 정보를 수집하였습니다. 현재 페이지에서는 키워드 당 100페이지만 수집하도록 조기종료 조건을 구현하였습니다.

 - https://cafe.naver.com/ca-fe/home/search/articles?q={KEYWORD}&p={PAGE}&em=1


### 2. 세부정보수집
 - '제목', '작성자', '일자', '댓글수', '좋아요개수', '조회수', '본문', 'url'를 수집하고자 합니다. 개요에서 말씀드린 것처럼 네이버 카페는 다시 접근하려고하면 오류가 발생합니다.
 - 이를 페이지의 탭변환을 통해 극복할 수 있습니다. 아래 코드가 이를 구현한 것입니다.

       change_tab = driver.window_handles[-1]
       driver.switch_to.window(change_tab)
       driver.switch_to.frame('cafe_main')    
       
 - 즉 수집은 url 접근 - 탭변환 - 정보수집 순서로 이루어집니다.


### 최종결과: 유럽여행이라는 키워드를 검색하여 얻은 결과입니다.

![image](https://user-images.githubusercontent.com/28617435/122663600-c8fa8580-d1d6-11eb-91d8-021d92019467.png)
