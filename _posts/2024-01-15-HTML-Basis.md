---
title: HTML 기초
date: 2024-01-15 00:00:00 +0900
categories: [HTML/CSS]
tags: [html]
--- 
- 이번 포스터에서는 `HTML`의 기본 개념과 문법에 대해서 알아보고자 한다.
- HTML에 대한 공부는 `공식 튜토리얼 문서`인 **w3schools**와 **MDN Web Docs**에서 하는 것이 가장 좋다.

## 1. HTML란?
- 웹 페이지를 만들기 위한 기본적인 구조를 정의하는 언어
- 하이퍼텍스트를 작성하고 구조화하는 언어
- <span style="color:red">웹 브라우저가 이해하고 해석</span>하여 사용자에게 보여주는 언어

## 2. HTML 기본 템플릿

```html
<!DOCTYPE html> 
<html lang="en"> 
  <head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0"> 
    <title> 제목</title>
  </head> 
  <body>
    웹 브라우저를 통해 보여지는 내용
  </body> 
</html>
```

## 3. HTML 문법
### 3.1. 제목
- `<h1>` ~ `<h6>`
- 숫자가 1일 때 가장 크고 6일 때 가장 작음

✍🏻 HTML 작성 시
```html
<h1> H1 Header </h1>
<h2> H2 Header </h2>
<h3> H3 Header </h3>
<h4> H4 Header </h4>
<h5> H5 Header </h5>
<h6> H6 Header </h6>
```
👉🏻 결과
![제목 결과입니다.](/assets/img/posts/2023-12-28/header_img.png)

### 3.2. 문단
- `<p>`
- 글의 본문을 표시할 때 주로 사용

✍🏻 HTML 작성 시
```html
<p>문단을 나타낼 때 사용하는 태그입니다.</p>
<p>하나의 문단에는 일정한 여백이 형성되어 화면 상에서 구분하기가 용이합니다.</p>
```
👉🏻 결과
<p>문단을 나타낼 때 사용하는 태그입니다.</p>
<p>하나의 문단에는 일정한 여백이 형성되어 화면 상에서 구분하기가 용이합니다.</p>

### 3.3. 인용구
- `<blockquote>`
- 일반적인 테스트보다 안쪽으로 들여써짐
- 출처가 있는 경우, `cite` 속성을 추가하여 작성하는 것이 좋음

✍🏻 HTML 작성 시
```html
<blockquote cite="https://www.naver.com">
출처는 네이버입니다.
</blockquote>
```
👉🏻 결과
<blockquote cite="https://www.naver.com">
출처는 네이버입니다.
</blockquote>

### 3.4. 그대로 보여주기
- `pre`

✍🏻 HTML 작성 시
```html
<pre>
오늘은 즐거운 일이 펼쳐질꺼야.
  벌써 오후잖아?
</pre>
```
👉🏻 결과
<pre>
오늘은 즐거운 일이 펼쳐질꺼야.
  벌써 오후잖아?
</pre>

### 3.5. 강조

✍🏻 HTML 작성 시
```html
<i> 이텔릭체 </i>
<em> 이텔릭체로 강조 </em>
<b> 진하게 표시 </b>
<strong> 진하게 강조 </strong>
```
👉🏻 결과<br>
<i> 이텔릭체 </i><br>
<em> 이텔릭체로 강조 </em><br>
<b> 진하게 표시 </b><br>
<strong> 진하게 강조 </strong><br>

### 3.6. display 요소
- `none` : 화면에서 사라짐
- `block` : 왼쪽에서 오른쪽으로 하나의 **가로 영역 전체를 차지**함
- `inline` : **자신이 둘러싸고 있는 내용들의 양에 따라** 길이가 결정되어 짐. **임의로 크기를 지정해 주어도 바뀌지 않음**.
- `inline-block` : 기본적으로 inline의 속성을 지니고 있지만, **임의로 크기 조정이 가능**함

✍🏻 HTML 작성 시
```html
<div style="display: none; background-color: blueviolet;"> display none </div>
<div style="display: block; background-color: aqua;"> display block </div>
<div style="display: inline; background-color: pink; width: 200px"> display inline </div>
<div style="display: inline-block; background-color: gold; width: 200px"> display inline-block </div>
```
👉🏻 결과<br>
<div style="display: none; background-color: blueviolet;"> display none </div>
<div style="display: block; background-color: aqua;"> display block </div>
<div style="display: inline; background-color: pink; width: 500px"> display inline </div>
<div style="display: inline-block; background-color: gold; width: 500px"> display inline-block </div>

### 3.7. 동작 관련 태그
#### 3.7.1. `<a>` 태그
- 하이퍼링크를 생성. href 속성을 통해 연결된 주소를 지정

✍🏻 HTML 작성 시
```html
<a href="https://www.naver.com">네이버 주소</a>
```
👉🏻 결과<br>
<a href="https://www.naver.com">네이버 주소</a>

#### 3.7.2. `<button>` 태그
- 클릭 가능한 버튼. 주로 JavaScript와 함께 사용되어 특정 동작을 수행

✍🏻 HTML 작성 시
```html
<button onclick="alert('Hello!')">클릭하세요</button>
```
👉🏻 결과<br>
<button onclick="alert('Hello!')">클릭하세요</button>

#### 3.7.3. `<input>` 태그
- 사용자 입력을 받는 입력 필드.
- type 속성을 통해 다양한 입력 필드를 만들 수 있음
  - text, password, number, email, checkbox, radio, file
- 다양한 속성을 통해 다양한 입력 필드를 만들 수 있음. 아래는 자주 사용할 것 같은 속성들이다.
  - value : 입력 필드의 값을 설정
  - placeholder : 입력 필드에 입력할 값의 예시
  - name : 입력 필드의 이름을 정의. 서버로 전송될 때 해당 이름으로 데이터가 전송됨
  - required : 필수 입력 필드로 설정합니다. 제출하기 전에 반드시 값을 입력해야 함
  - maxlength : 입력 필드에 입력 가능한 최대 문자 수를 지정
  - min, max : 숫자 입력 필드의 최소 및 최대값을 지정
  - pattern : 입력 필드에 입력할 수 있는 정규 표현식 패턴을 지정

✍🏻 HTML 작성 시
```html
<input type="text" name="username" maxlength=20 placeholder="이름"/>
```
👉🏻 결과<br>
<input type="text" name="username" maxlength=20 placeholder="이름"/>

#### 3.7.4. `<label>` 태그
- 입력 요소에 대한 설명을 제공하거나, 사용자가 클릭 가능한 레이블을 생성한다.

✍🏻 HTML 작성 시
```html
<label for="name">이름</label>
<input type="text" id="name" name="name"/>
```
👉🏻 결과<br>
<label for="name">이름</label>
<input type="text" id="name" name="name"/>

#### 3.7.5. `<textarea>` 태그
- 여러 줄의 텍스트를 입력할 수 있는 입력 필드를 생성

✍🏻 HTML 작성 시
```html
<textarea rows="4" cols="50" placeholder="메시지를 입력하세요"></textarea>
```
👉🏻 결과<br>
<textarea rows="4" cols="50" placeholder="메시지를 입력하세요"></textarea>

#### 3.7.6. `<form>` 태그
- 데이터를 서버로 제출하는 양식을 생성함.
- 주로 버튼이나 입력 필드들을 담음
- `action` 속성 : 입력된 데이터가 전송될 서버 쪽 URL을 지정
- `method` 속성 : 입력된 데이터를 서버로 전송하는 방식을 결정
  - 주로 **GET**과 **POST**를 사용
  - **GET** : URL에 데이터를 추가해서 전송
  - **POST** : 숨겨진 방식으로 데이터 전송

✍🏻 HTML 작성 시
```html
<form action="#" method="post">
  <input type="text" name="username" placeholder="사용자명">
  <input type="password" name="password" placeholder="비밀번호">
  <button type="submit">제출</button>
</form>
```
👉🏻 결과<br>
<form action="#" method="post">
  <input type="text" name="username" placeholder="사용자명">
  <input type="password" name="password" placeholder="비밀번호">
  <button type="submit">제출</button>
</form>

#### 3.7.6. `<slect>` 태그
- 드롭다운 목록을 생성. 사용자가 옵션을 선택할 수 있음

✍🏻 HTML 작성 시
```html
<select>
  <option value="1">항목 1</option>
  <option value="2">항목 2</option>
  <option value="3">항목 3</option>
</select>
```
👉🏻 결과<br>
<select>
  <option value="1">항목 1</option>
  <option value="2">항목 2</option>
  <option value="3">항목 3</option>
</select>

#### 3.7.7. `<fieldset>` 태그
- 여러 관련된 입력 요소들을 그룹화
- 주로 `<legend>`와 함께 사용

✍🏻 HTML 작성 시
```html
<fieldset>
  <legend>개인 정보</legend>
  <label for="name">이름:</label>
  <input type="text" id="name" name="name"><br>
  <label for="email">이메일:</label>
  <input type="email" id="email" name="email"><br><br>
</fieldset>
```
👉🏻 결과<br>
<fieldset>
  <legend>개인 정보</legend>
  <label for="name">이름:</label>
  <input type="text" id="name" name="name"><br>
  <label for="email">이메일:</label>
  <input type="email" id="email" name="email"><br><br>
</fieldset>

#### 3.7.8. `<iframe>` 태그
- 다른 HTML 페이지를 현재 페이지에 삽입하는데 사용

✍🏻 HTML 작성 시
```html
<iframe src="/posts/CSS-Basis/" width="600" height="400"></iframe>
```
👉🏻 결과<br>
<iframe src="/posts/CSS-Basis/" width="600" height="400"></iframe>

### 3.8. 미디어 관련 태그들
#### 3.8.1. `<img>` 태그
- 이미지를 삽입하는 태그로, src 속성을 통해 이미지 파일의 경로를 지정

✍🏻 HTML 작성 시
```html
<img src="/assets/img/posts/2024-01-15/burby.png" alt="이미지 예시" width="200">
```
👉🏻 결과<br>
<img src="/assets/img/posts/2024-01-15/burby.png" alt="이미지 예시" width="200">

#### 3.8.2. `<audio>` 태그
- 오디오를 재생하는 태그로, 소리 파일을 삽입하고 컨트롤

✍🏻 HTML 작성 시
```html
<audio controls>
  <source src="/assets/img/posts/2024-01-15/sample_audio.mp3" type="audio/mpeg">
</audio>
```
👉🏻 결과<br>
<audio controls>
  <source src="/assets/img/posts/2024-01-15/sample_audio.mp3" type="audio/mpeg">
</audio>

#### 3.8.3. `<video>` 태그
- 비디오를 재생하는 태그로, 동영상 파일을 삽입하고 컨트롤

✍🏻 HTML 작성 시
```html
<video controls width="300" muted>
  <source src="/assets/img/posts/2024-01-15/sample_video.mp4" type="video/mp4">
</video>
```
👉🏻 결과<br>
<video controls width="300" muted>
  <source src="/assets/img/posts/2024-01-15/sample_video.mp4" type="video/mp4">
</video>

### 3.9. 목록 태그
#### 3.9.1. 정렬 리스트
- `<ul>` : 순서가 없는 리스트. 각 아이템을 점이나 기호로 표시됨
- `<ol>` : 순서가 있는 리스트. 각 아이템을 숫자로 표시됨
- `<li>` : 리스트 아이템을 가리킴

✍🏻 HTML 작성 시
```html
<ul>
  <li> 순서 없는 첫 번째</li>
  <li> 순서 없는 두 번째</li>
  <li> 순서 없는 세 번째</li>
</ul>
<ol>
  <li> 순서 있는 첫 번째</li>
  <li> 순서 있는 두 번째</li>
  <li> 순서 있는 세 번째</li>
</ol>
```
👉🏻 결과<br>
<ul>
  <li> 순서 없는 첫 번째</li>
  <li> 순서 없는 두 번째</li>
  <li> 순서 없는 세 번째</li>
</ul>
<ol>
  <li> 순서 있는 첫 번째</li>
  <li> 순서 있는 두 번째</li>
  <li> 순서 있는 세 번째</li>
</ol>

#### 3.9.2. 정의 리스트
- `<dl>` : 설명 리스트를 만들 때 사용됨. 주로 용어나 정의들을 표시할 때 쓰임
- `<dt>` : 정의 리스트에서 용어를 정의할 때 사용
- `<dd>` : 정의 리스트에서 정의에 해당하는 내용을 표시할 때 사용

✍🏻 HTML 작성 시
```html
<dl>
  <dt>ISFJ(수호자)</dt>
  <dd>헌신적이며 굉장히 상냥하고, 책임감도 강해서 다른 사람의 버티목이 될 수 있는 타입니다.</dd>
</dl>
```
👉🏻 결과<br>
<dl>
  <dt>ISFJ(수호자)</dt>
  <dd>헌신적이며 굉장히 상냥하고, 책임감도 강해서 다른 사람의 버티목이 될 수 있는 타입니다.</dd>
</dl>

### 3.10. 시멘틱 태그
- `<header>` : 웹 페이지나 섹션의 헤더를 정의
- `<footer>` : 웹 페이지나 섹션의 푸터를 정의
- `<nav>` : 네비게이션 링크를 담는 태그
- `<article>` : 독립적인 콘텐츠 영역을 정의
- `<section>` : 문서의 섹션을 정의하며 주제별로 그룹화함
- `<aside>` : 본문 컨텐츠와는 관련성이 떨어지는 사이드바를 정의
- `<main>` : 문서의 주요 콘텐츠를 감싸는 태그로, 페이지에 단 하나만 사용
- `<figure>` : 이미지, 차트, 탭션 등의 그룹을 정의
- `<figcaption>` : `<figure>` 요소의 캡션을 정의
- `<time>` : 날짜, 시간을 나타냄

## 마치며
- HTML 문법은 전에 몇 번 사용해 봤기에 정리하는 마음으로 이 포스터를 작성했다.
<!-- TODO javascript 학습 이후 canvas 추가 -->
- `<canvas>` 같은 경우에는 추후에 시간이 된다면 업데이트할 예정이다.

## 참고 자료
> **신한투자증권 프로 디지털 아카데미 3기**