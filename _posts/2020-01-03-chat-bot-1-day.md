---
title: "Chat bot 1 Day"
categories:
  - dev
tags:
  - chatbot
  - crawling
  - python
  - bs4
---

# UserAgent

~~~ 
import requests
from bs4 import BeautifulSoup
from fake_useragent import UserAgent

ua = UserAgent()
header = {'user-agent':ua.chrome}

url = 'http://codingbat.com'
page = requests.get(url+'/java',headers = header)
soup = BeautifulSoup(page.text, 'lxml') 
~~~

사이트에서 request시 UserAgent 라이브러리의 fake_useragent 모듈을 사용

대부분의 사이트에서 크롤링시 봇이라고 인지되는 행동을 하면 차단한다.
그래서 예전에 크롤링할때는, time slice를 자체적으로 두어(사람인척하여) 사이트에서 차단되는 것을 방지했었다.
이를 fake_useragent를 사용하여 자체적으로 시간을 delay할 필요 없어졌다.

# 까다로운 html

![image](https://user-images.githubusercontent.com/31649100/72257670-892d8700-364f-11ea-8a09-4936b500b58c.png)
여기서 sleepIn~ 부분을 크롤링하는 것이 목표

처음에는 .minh를 select하여 div의 next_sibling을 반복하여 crawling 했었다.
대부분은 paremeter가 2개이고 시 결과 값은 3개가 존재한다. 
하지만 parameter(boolean, boolean)가 2개가 아닌 1개를 가진 함수가 다른 페이지에서 존재하였고 error가 발생하였다.

이를 해결하기 위해, type 비교를 시도하였다.
bs4.element.NavigableString 코드에서 bs4를 읽지 못하여 error가 발생하였고, 이는 해결하지 못 하였다.

~~~
for example in examples :
      children = [child for child in example.parent if child != '\n']
~~~
.minh에서 parent로 올라가 children의 개수를 통해 index로 출력하여 해결하였다.

![image](https://user-images.githubusercontent.com/31649100/72257631-71ee9980-364f-11ea-84a6-3c899dd8b798.png)


