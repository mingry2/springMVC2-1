# 🔥 springMVC2
## 목표
- Thymeleaf 기본 기능 파악
---
## Thymeleaf(타임리프)
📢 [공식 사이트](https://www.thymeleaf.org/)

📢 [공식 메뉴얼](https://www.thymeleaf.org/doc/tutorials/3.0/usingthymeleaf.html)
* HTML 템플릿 엔진으로 주로 자바 기반 웹 애플리케이션에서 자주 사용
* 서버에서 데이터를 동적으로 생하고 이를 HTML에 통합하여 사용자에게 동적인 웹 페이지를 제공(서버-사이드 렌더링/SSR)
* 네츄럴 템플릿으로 순수 HTML을 최대한 유지
  * 웹 브라우저에서 파일을 직접 열어도 HTML 코드가 깨지지 않고 내용을 확인할 수 잇고, 서버를 통해 뷰 템플릿을 거치면 동적으로 변경된 결과도 확인할 수 있음
## Thymeleaf 기본 기능
✨ 타임리프 사용 선언
```html
<html xmlns:th="http://www.thymeleaf.org">
```
### 1. 기본 표현식
- 간단한 표현
  - 변수 표현식 : ```${...}```
  - 선택 변수 표현식 : ```*{...}```
  - 메시지 표현식 : ```#{...}```
  - 링크 URL 표현식 : ```@{...}```
  - 조각 표현식 : ```~{...}```
- 리터럴
  - 텍스트 : 'one text', 'Another one!' / 홑따옴표를 꼭 입력
  - 숫자 : 0, 34, 1 , ...
  - 불린 : true, false
  - 널 : null
  - 리터럴 토큰 : one, sometext, main, ... 
- 문자 연산
  - 문자 합치기 : ```+```
  - 리터럴 대체 : ```|The name is ${name}|```
- 산술 연산
  - Binary operators : +, -, *, /, %
  - Minus sign (unary operator) : -
- 불린 연산
  - and, or
  - !, not
- 비교와 동등
  - 비교 : >,<,>=,<=(gt,lt,ge,le)
  - 동등 연산 : ==,!=(eq,ne)
- 조건 연산
  - if-then : if ? then
  - if-then-else : if ? then : else
  - default : value ?: default-value
### 2. 텍스트 (text, utext)
- 타임리프는 기본적으로 HTML 태그의 속성에 기능을 정의하여 동작한다.
```html
<span th:text="${data}"
```
- 태그 속성이 아닌 HTML 콘텐츠 영역 안에서 직접 데이터 출력
```html
[[${data}]]
```
- Escape
  - 웹 브라우저는 ```<```, ```>```을 기본적으로 태그의 시작과 끝으로 인식
  - 태그의 시작과 끝이 아닌 문자로 표현하는 것을 ```HTML 엔티티```라고함
  - HTML 태그 -> HTML 엔티티 변경하는 것을 이스케이프(Escape)라고함
  - 타임리프는 기본적으로 이스케이프를 제공
  - ```<``` -> ```&lt;```
- Unescape
  - 이스케이프 기능을 제거
    - ```th:untext``` or ```[()]``` 사용
### 3. 변수 - SpringEL
```html
<span th:text="${user.username}"></span></li>
```
- Object, List, Map 등 다양하게 변수 표현식을 사용할 수 있음
 ```html
user['username'] <!-- Object -->
users[0].username <!-- List -->
userMap['userA'].username <-- Map -->
```
### 4. 지역 변수 선언
```html
 <div th:with="first=${users[0]}">
    <p>처음 사람의 이름은 <span th:text="${first.username}"></span></p>
 </div>
```
### 5. 기본 객체들
- request, response, session, servletContext는 Controller에서 model로 전달 받아 사용해야함(스프링 부트 3.0 부터 해당 객체들을 지원하지 않는다.)
```html
<li>request = <span th:text="${request}"></span></li>
<li>response = <span th:text="${response}"></span></li>
<li>session = <span th:text="${session}"></span></li>
<li>servletContext = <span th:text="${servletContext}"></span></li>
<li>locale = <span th:text="${#locale}"></span></li>
```
- 편의 객체
  - reqeust.getParameter("data") 이런식으로 접근해야하기 때문에 힘들어짐 -> 편의 객체를 제공
  - 스프링 빈 이름으로 직접 접근도 가능
```html
<li>Request Parameter = <span th:text="${param.paramData}"></span></li>
<li>session = <span th:text="${session.sessionData}"></span></li>
<li>spring bean = <span th:text="${@helloBean.hello('Spring!')}"></span></li>
```
### 6. 유틸리티 객체와 날짜
- ```#message``` : 메시지, 국제화 처리
- ```#uris``` : URI 이스케이프 지원
- ```#dates``` : java.util.Date 서식 지원
- ```#calendars``` : java.util.Calendar 서식 지원
- ```#temporals``` : 자바8 날짜 서식 지원
- ```#numbers``` : 숫자 서식 지원
- ```strings``` : 문자 관련 편의 기능
- ```#objects``` : 객체 관련
- ```#bools``` : boolean 관련
- ```#arrays``` : 배열 관련
- ```#lists,#sets,#maps``` : 컬렉션 관련
### 7. URL 링크
- 쿼리 파라미터, 경로 변수 , 쿼리+경로 변수 모두 지원
```html
<li><a th:href="@{/hello}">basic url</a></li>
<li><a th:href="@{/hello(param1=${param1}, param2=${param2})}">hello query param</a></li>
<li><a th:href="@{/hello/{param1}/{param2}(param1=${param1}, param2=${param2})}">path variable</a></li>
<li><a th:href="@{/hello/{param1}(param1=${param1}, param2=${param2})}">path variable + query parameter</a></li>
```
### 8. 리터럴
- 타임리프에서 문자 리터럴은 항상 ```'(작은 따옴표)```로 감싸야한다.
```html
<span th:text="'hello'">
```
- 공백 없이 쭉 이어진다면 하나의 의미있는 토큰으로 인지해서 다음과 같이 작은 따옴표를 생략할 수 있다.
  - A-Z, a-z, 0-9, [], ., -, _
```html
<span th:text="hello">
```
- 리터럴 대체
  - 리터럴 대체 문법 ```||```을 사용하면 마치 템플릿을 사용하는 것처럼 편리하다.
```html
<span th:text="|hello ${data}|">
```
### 9. 연산
- HTML 엔티티(gt,lt,ge,le,...)을 주의해서 연산
- 자바와 연산 방법은 비슷하다.
- 비교 연산
```html
<li>1 > 10 = <span th:text="1 &gt; 10"></span></li>
<li>1 gt 10 = <span th:text="1 gt 10"></span></li>
<li>1 >= 10 = <span th:text="1 >= 10"></span></li>
<li>1 ge 10 = <span th:text="1 ge 10"></span></li>
<li>1 == 10 = <span th:text="1 == 10"></span></li>
<li>1 != 10 = <span th:text="1 != 10"></span></li>
```
- 조건식
```html
<li>(10 % 2 == 0)? '짝수':'홀수' = <span th:text="(10 % 2 == 0)? '짝수':'홀수'"></span></li>
```
- Elvis 연산자(조건식의 편의 버전)
```html
<li>${data}?: '데이터가 없습니다.' = <span th:text="${data}?: '데이터가 없습니다.'"></span></li>
<li>${nullData}?: '데이터가 없습니다.' = <span th:text="${nullData}?: '데이터가 없습니다.'"></span></li>
```
- No-Operation
  - ```-```인 경우 타임리프가 실행되지 않는 것 처럼 동작
  - HTML의 내용 그대로 활용 가능
```html
<li>${data}?: _ = <span th:text="${data}?: _">데이터가 없습니다.</span></li>
<li>${nullData}?: _ = <span th:text="${nullData}?: _">데이터가 없습니다.</span></li>
```
### 10. 속성 값 설정
- ```attrappend```, ```attrprepend```를 이용하면 원하는 속성값에 원하는 값을 추가할 수 있다.
- ```classappend```를 이용하면해당 속성에 자연스럽게 추가한다.
- ```checked```처리
  - HTML에서는 checked 속성이 있으면 무조건 checked 처리가 되어버린다.
  - 타임리프는 ```th:checked="false"```인 경우 속성 자체를 삭제한다.
### 11. 반복
- ```th:each```를 사용하여 반복
```html
<tr th:each="user : ${users}">
    <td th:text="${user.username}">username</td>
    <td th:text="${user.age}">0</td>
</tr>
```
- ```000+Stat```을 사용하면 반복의 상태를 확인할 수 있음(생략가능)
  - index, count, size, even/odd, first/last, current
```html
<tr th:each="user, userStat : ${users}">
```
### 12. 조건부 평가
- ```if```, ```unless(if의 반대)```
  - 해당 조건이 맞지 않으면 태그 자체를 렌더링하지 않는다.
```html
<span th:text="${user.age}">0</span> 
<span th:text="'미성년자'" th:if="${user.age lt 20}"></span>
<span th:text="'미성년자'" th:unless="${user.age ge 20}"></span>
```
- ```switch```
  - ```*``` 조건이 없을 때 사용
```html
<td th:switch="${user.age}"</td>
    <span th:case="10">10살</span>
    <span th:case="20">20살</span>
    <span th:case="*">기타</span>
```
### 13. 주석
- 표준 HTML 주석은 타임리프가 렌더링하지 않고 그대로 남겨둔다
- 타임리프 파서 주석은 타임리프의 진짜 주석으로 렌더링에서 주석 부분을 제거한다.
- 타임리프 프로토타입 주석은 HTML 파일을 웹 브라우저에서 그대로 열면 웹 브라우저가 렌더링하지 않는다. 하지만, 타임리프 렌더링을 거치면 정상 렌더링이된다.
```html
<h1>1. 표준 HTML 주석</h1>
<!--
<span th:text="${data}">html data</span>
-->
<h1>2. 타임리프 파서 주석</h1>
<!--/* [[${data}]] */-->
<!--/*-->
<span th:text="${data}">html data</span>
<!--*/-->
<h1>3. 타임리프 프로토타입 주석</h1>
<!--/*/
<span th:text="${data}">html data</span>
/*/-->
```
### 14. 블록
- ```<th:block>```은 HTML 태그가 아닌 타임리프의 유일한 자체 태그이다.
```html
<th:block th:each="user : ${users}">
    <div>
        사용자 이름1 <span th:text="${user.username}"></span>
        사용자 나이1 <span th:text="${user.age}"></span>
    </div>
    <div>
        요약 <span th:text="${user.username} + ' / ' + ${user.age}"></span>
    </div>
 </th:block>
```
- 위 코드와 같이 div 태그 2군데를 함께 반복처리하고 싶을 때 블록을 사용하여 반복한다.
### 15. 자바스크립트 인라인
- 타임리프는 자바스크립트에서 타임리프를 편리하게 사용할 수 있는 자바스크립트 인라인 기능을 제공한다.
```javascript
<script th:inline="javascript">
        var username = [[${user.username}]];
        var age = [[${user.age}]];
        //자바스크립트 내추럴 템플릿
        var username2 = /*[[${user.username}]]*/ "test username";
        //객체
        var user = [[${user}]];
</script>
```
### 16. 템플릿 조각
### 17. 템플릿 레이아웃