## 네이버 플레이스 크롤링입니다. (bs4)
- 이번 파트에서는 bs4를 활용해 네이버 플레이스를 크롤링하고자 합니다. 기본적인 url 형태는 아래와 같습니다. 아래 {}에는 placeid가 들어갑니다.

https://m.place.naver.com/place/{}/home?entry=ple

- placeid가 무엇이지? 이는 <네이버 지도> 크롤링을 참고하시면 됩니다. 각 업체별로 네이버는 고유의 id를 제공하는데, 이것을 placeid라고 명명하였습니다. 
  > <네이버 지도> 크롤링 당시 가져온 id일부로 크롤링을 하였습니다. ['1106610041', '1078930793', '36543606']

- bs4는 되게 원시적입니다. 원하는 element로 정보를 가져올 수 있으며, 이를 str형식으로 변환해 slicing을 거쳐 정보를 가져옵니다다.
  > value7~9는 각각 주소, 가맹점명, 카테고리를 의미합니다.
  > ![image](https://user-images.githubusercontent.com/28617435/122668040-3a473200-d1f1-11eb-89e6-6c79d84b9a83.png)


## 주의해야할 점
 - 네이버 플레이스는 2가지 url로 이루어져 있습니다. 만약 첫번째 url로 접근이 안된다면, 2번째 url로 시도합시다. 그래도 몇개는 더 건질 수 있습니다.
 
       https://m.place.naver.com/place/{}/home?entry=ple
       https://m.place.naver.com/accommodation/{}


