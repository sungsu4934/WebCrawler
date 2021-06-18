## 네이버쇼핑몰을 크롤링하는 코드입니다.

> 저는 네이버 쇼핑몰 리뷰를 크롤링하는 것에 초점을 맞추었습니다. NLP에 리뷰와 같은 데이터가 필요하신 분은 유용하게 쓰실 수 있을 것입니다.
 *https://search.shopping.naver.com/search/allfrm=NVSHMDL&origQuery=%EC%9A%B0%EC%9C%A0&pagingIndex=1&pagingSize=40&productSet=model&query=%EC%9A%B0%EC%9C%A0&sort=review&timestamp=&viewType=list*

> 네이버 쇼핑몰 리뷰 크롤링은 lxml로 url을 우선적으로 크롤링한 후 selenium으로 세부 리뷰들을 크롤링하는 식으로 진행됩니다.

## 수집과정 (1. url 수집)

Step1. 수집할 url의 형태는 다음과 같다. 각 수집할 때 주의해야할 점은 아래와 같다.

https://search.shopping.naver.com/search/all?exagency=true&frm=NVSHMDL&origQuery={KEYWORD}&pagingIndex={PAGE}&pagingSize=5&productSet=model&query={KEYWORD}&sort=review&timestamp=&viewType=list

  > a. KEYWORD는 라이브러리 내 세부 키워드들이 들어간다.
  > 
  > b. PAGE는 현재 아래 보이는 것과 같은 1~6를 조정하기 위해 들어간다.
  > 
  > c. pagingSize인자에 5를 대입하였다. 이는 내가 한 페이지에서 볼 수 있는 최대 항목수를 나타낸다. 스크롤을 내리지 않으면 최대 5개의 정보만 제공하여 pagingSize에 40을 넣더라도 5개의 정보만 수집된다. selenium으로 구현하여 스크롤을 내리게 할 수 있으나 스크롤을 내리는데 오류가 발생할 위험이 있고 lxml로 5개씩 여러 페이지를 수집하는 것이 빠르다고 판단하여 pagingSize는 5로 고정하였다.

Step2. 본격적인 수집
본격적으로 위 url에대해 정보를 수집해본다. 정보수집을 위한 알고리즘은 아래와 같이 작성하였다.



     for 키워드 in 검색하고자하는 단어들
  
         키워드 검색
         While True: # url 수집단계
           정보수집 (만약 리뷰개수가 0인 항목이 있다면 0을 채워줄 것)

           # Stopping condition 2가지
           If 상품명이 수집되지 못하였다면 --> 오류일 수 있으니 재시도
              재시도 후에도 안되면 정보가 없는 페이지로 판단하여 해당 키워드는 수집종료
              While문 break
           If 해당 url에 리뷰개수가 0인 항목을 발견 --> 이하 url은 수집하지 않음 
              *그 뒤 url은 모두 리뷰가 0개일 것이기 때문*

           데이터프레임에 url에서 수집한 데이터를 저장(5개씩)
 

 

-	수집할 대상은 “상품명, 등록일, 리뷰개수, 상품url, 검색키워드”이다. 

- 조기종료 조건은 2가지다. 

        a. 우선적으로 lxml에서 계속 url을 불러올 경우 순간 url을 받아오지 못할 때가 있다. 해당 경우를 판단하는 기준은 상품명이 제대로 수집되었나로 파악하였다. 
        이런 케이스에 대해 1차적으로 10초 휴식 후 다시 접근하도록 한다. 만약 재접근을 했음에도 상품명이 수집되지 않았다면 해당 페이지는 정말 상품이 없어서 
        상품명이 수집되지 않은 것으로 판단하여 조기종료를 실시한다.

        b. 우리는 페이지를 리뷰많은순으로 정렬하였다. 즉, 리뷰가 0개인 곳을 발견했다면 뒤 페이지는 수집할 필요가 없다. 
        리뷰가 0개인 페이지를 발견한다면 해당 키워드는 조기종료하고 다음 키워드로 넘어간다.


