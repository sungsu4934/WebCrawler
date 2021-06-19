## 보배드림 커뮤니티 게시글을 크롤링하는 코드입니다. (selenium + lxml)

> 총 2가지 코드를 준비해보았습니다. 
>> 1. 아래 링크에 접속한 후, 특정 키워드를 검색한 후 커뮤니티 전체에서 검색하여 리뷰수집 (키워드를 검색해도 url이 안바뀌기에 selenium필요)

https://www.bobaedream.co.kr/search 

>> 2. 베스트글 전체를 크롤링 --> page인자만 바꿔가면서 이후 페이지를 수집

https://www.bobaedream.co.kr/list?code=best&s_cate=&maker_no=&model_no=&or_gu=10&or_se=desc&s_selday=&pagescale=30&info3=&noticeShow=&s_select=&s_key=&level_no=&bestCode=&bestDays=&bestbbs=&vdate=&type=list&page=2


## 1. 커뮤니티 전체에서 특정 키워드에 관한 내용 검색 후, 해당 리뷰를 크롤링
- 수집절차
   1. url수집 (selenium)
   2. 세부정보수집 (lxml)

- url수집
 > url수집에 있어 아래 링크에 들어간 후, 특정 키워드를 검색하고 커뮤니티를 누릅니다. url이 변경되지 않기에 동적크롤링이 필요합니다. (selenium)
 
 https://www.bobaedream.co.kr/search 
 
 > 크게 주의할 것은 없으나, 페이지 넘기는 것을 주의합니다. "이전 1 2 3 4 5 다음 >" 과 "< 이전 11 12 13 14 15 >" 과 "< 이전 55 56 57" 의 xpath가 동일한지 안한지 유심히 볼 필요가 있습니다. 본 코드에서는 이를 예외처리 하는 것에 많은 신경을 썼습니다.


- 세부정보수집

 > lxml을 활용해 1번단계에서 수집한 url에 순차적으로 접근하여 정보를 수집합니다. 게시글의 제목과 본문만 추가적으로 크롤링하였습니다. lxml이더라도 자주 접근하면 순간적으로 다운될 수 있기에 만약 에러가 떴다면 기다렸다가 다시 접근하는 것을 구현하였습니다.
