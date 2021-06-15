# Semantic Web 이란?

## Semantic Web 이란?
- 시멘틱 웹이란 의미론적인 웹이란 뜻으로 문서 즉, 어플리케이션의 의미에 맞게 구성하는 웹이다.   

```
컴퓨터가 사람을 대신하여 정보를 읽고, 이해하고 가공하여 새로운 정보를 만들어 낼 수 있도록 이해하기 쉬운 의미를 가진 차세대 지능형 웹.
또한 다른 의미로는 정보를 분석하여 그 정보의 관계 속에서 의미론적인 자료들을 추출하여 웹 상에 보여줄 수 있는 웹 이다.
```   

- 2가지 예시를 들어 시멘틱 웹의 개념을 이해 해보자   
   
```
// 1번
<b>Welcome to the workspace</b>

// 2번
<h1>Welcome to the workspace</h1>
```   
- 1번 같은 경우 의미론적으로 어떤 의미도 가지고 있지 않다. 즉, 개발자가 의도한 요소의 의미를 명확하게 나타내지 않고 있다.
- 그러나, 2번의 경우 header(제목) 중 가장 상위 레벨이라는 의미를 내포하고 있어서 개발자가 의도한 요소의 의미가 명확히 드러나고 있다.
- 이것은 코드의 가독성을 높이고 유지보수를 쉽게 해준다.   
   
## HTML 요소(element)의 2가지 타입
- HTML 요소는 non-semantic 요소, semantic 요소로 구분
    1. non-semantic 요소div, span 등이 있으며 이들 태그는 content에 대하여 어떤 설명도 하지 않는다.
    2. semantic 요소 form, table, img 등이 있으며 이들 태그는 content의 의미를 명확히 설명한다.   
   
## HTML5 에서 새롭게 추가된 시맨틱 태그들   
   
```
<body>
  <header>
     <h1>헤더</h1>
  </header>

<nav>
   <ul>
      <li><a href="#"> A </a></li>
      <li><a href="#"> B </a></li>
      <li><a href="#"> C </a></li>
   </ul>
</nav>

<section>
  <article>
    <h1>제목1</h1>
    <p>내용1</h1>
  </article>
  <article>
    <h1>제목2</h1>
    <p>내용2</p>
  </article>
</section>

<footer>
  <address>주소</address>
</footer>

</body>
```      
   
<img src="./../Image/Semantic%20Web.png" width="700px" height="550px" title="px(픽셀) 크기 설정" alt="Semantic Web"></img>