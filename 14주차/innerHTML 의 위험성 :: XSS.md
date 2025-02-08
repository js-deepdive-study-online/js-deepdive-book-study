# 39장:: innerHTML 의 위험성 :: XSS

> 📅 스터디 날짜: 2024년 12월 30일

> 💡 사용자로 부터 입력받은 데이터를 그대로 `innerHTML` 를 통해 할당하면,   
> XSS 공격에 취약하기에 위험하다!

## innerHTML 란?  
innerHTML은 HTML 콘텐츠를 설정하거나 가져올 때 사용한다.  
innerHTML 프로퍼티를 사용하면 HTML 마크업 문자열로 간단히 DOM 조작이 가능하다.

```jsx
const element = document.querySelector(".hello");

element.innerHTML = "abc";
```

HTML 문자열을 DOM에 바로 주입 —> **악성 스크립트**가 포함될 경우, **그대로 실행**되기에 주의 !!

## XSS란?  
Cross-Site Scripting  
사용자 입력값을 제대로 검증하거나 필터링하지 않아, 악성 스크립트가 실행되는 취약점을 악용한 공격  
공격자가 다른 사용자의 브라우저에서 **악성 JavaScript 코드**를 실행하도록 만들어 민감한 정보를 탈취하거나 사용자의 세션을 가로챌 수 있도록 한다  
→  HTML 마크업 내에 자바스크립트 악성 코드가 포함되어 있다면 **파싱 과정에서 그대로 실행**될 가능성이 있기 때문

## XSS 영향  
- 피싱
    - 사용자 인터페이스 조작
    - 악성 사이트로 리다이렉션 (ex. 로그인 페이지)
- 멀웨어 배포
    - 브라우저에 악성 소프트웨어 다운로드 or 설치 유도
- 사용자 데이터 가로채기
    - 사용자 입력 폼에서 민감한 데이터 가로챔

## XSS 사례
- 네이버 웹툰 댓글 테러 사건
- 뽐뿌 개인정보 유출 사건

## XSS 주요 유형

### - Stored XSS (저장형 XSS)

> 공격자가 악성 스크립트를 **서버에 저장**하고, 다수의 사용자가 볼 수 있는 페이지에서 실행되도록 한다

1. 공격자가 댓글 입력란에 `<script>alert('XSS!');</script>`를 삽입
2. 서버는 이를 필터링 없이 저장하고, 다른 사용자가 댓글을 볼 때 스크립트가 실행

→ 다수의 사용자에게 영향 + 지속적

```jsx
// 서버에서 저장된 사용자 댓글을 출력하는 코드
const comments = [
	{ user: "Alice", comment: "<img src='x' onerror='alert(1)'>" },
  { user: "Bob", comment: "Hello, world!" },
];

const commentsContainer = document.getElementById("comments");

// 댓글을 HTML로 출력
comments.forEach(({ user, comment }) => {
  commentsContainer.innerHTML += `<p><strong>${user}:</strong> ${comment}</p>`;
});
```

기존에는 `script` tag 가 삽입될 경우 적용이 되었지만  
HTML5 는 `innerHTML` 과 함께 삽입된 `script` tag는 실행되지 않도록 한다!  
하지만 `img` tag 에서 `onerror` 로 여전히 사용할 수 있다

### - Reflected XSS (반사형 XSS)

> 쿼리스트링에 스크립트를 주입하여, 서버를 거치지 않고 응답 페이지에서 바로 실행되도록 함

1. 공격자가 URL에 악의적입 스크립트를 담아 공유
   `http://example.com/search?q=<script>alert('XSS!')</script>`
2. 사용자는 스크립트가 삽입된 페이지로 연결
3. 스크립트가 실행

→ 특정 사용자에게만 영향

ex) 공개 게시판, 피싱 이메일, 단축 URL, 실제와 유사한 URL

### - DOM-based XSS

> 클라이언트 **Javascript 코드가 취약점**을 가지고 있을 때 발생 (서버 개입x)
코드가 DOM에서 **사용자 입력값을 그대로 처리**할 때 악성 스크립트 실행

1. 브라우저에서 `document.write(window.location.hash)`와 같은 코드를 사용하면, URL 해시 값에 포함된 스크립트가 실행

→ 클라이언트가 코드 수정하지 않는 이상, 취약점 유지

## 대응방안

### 문자열 주입시, `textContent` 사용

- DOM으로 삽입되지 않고, 단순 텍스트로 출력

```jsx
const userInput = "<script>alert('XSS!');</script>";
const container = document.getElementById("container");

// 안전하게 텍스트 추가
container.textContent = userInput;
```

### 문자열 주입시, HTML 이스케이프 처리

- 특수 문자를 HTML 이스케이프로 처리하여, HTML 태그로 해석되지 않도록 처리

```jsx
function escapeHTML(str) {
  return str
    .replace(/&/g, "&amp;")
    .replace(/</g, "&lt;")
    .replace(/>/g, "&gt;")
    .replace(/"/g, "&quot;")
    .replace(/'/g, "&#039;");
}

const userInput = "<script>alert('XSS!');</script>";
const container = document.getElementById("container");

// 안전하게 이스케이프된 문자열 삽입
container.innerHTML = escapeHTML(userInput);
```

### 요소 주입시, `DOMPurify` 라이브러리 사용

- XSS 방지 라이브러리
- HTML 문자열에서 안전하지 않은 부분 제거

```jsx
const userInput = "<script>alert('XSS!');</script>";
const container = document.getElementById("container");

// DOMPurify로 안전하게 HTML 처리
const safeHTML = DOMPurify.sanitize(userInput);
container.innerHTML = safeHTML;
```

- 쿠키에 HttpOnly 옵션 활성화
    - 쿠키 값을 스크립트로 탈취할 수 없도록, 서버단에서 설정

cf)

https://developer.mozilla.org/ko/docs/Web/API/Element/innerHTML  
https://velog.io/@rachaen/JavaScript-%ED%81%AC%EB%A1%9C%EC%8A%A4%EC%82%AC%EC%9D%B4%ED%8A%B8-%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8XSS  
https://velog.io/@pock11/innerHTML-dangerouslySetInnerHTML  
https://velog.io/@kylexid/XSS-Cross-Site-Scripting