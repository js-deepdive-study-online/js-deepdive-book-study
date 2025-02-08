# 26장 - 30장:: ECMAScript란?

> 📅 스터디 날짜: 2024년 11월 26일

`ECMA International` 에 의해 `ECMA-262` 기술 규격에 따라 정의된 범용 스크립트 언어 (= ES)

- ex)
    - 일상 생활에서 쓰는 언어의 기준이 되는 국어(= 표준어) → 국립국어원이 관리
        - 국립국어원이 제정한 여러 규칙(발음, 맞춤법)의 원리를 따른다.
    - 비교
        - 국립국어원 = ECMA 인터네셔널
        - 표준어 = ECMA-262
        - 맞춤법과 같은 규칙 = ECMAScript
- JavaScript
    - ECMAScript 사양을 준수하는 범용 스크립팅 언어


## 누가 결정하고 만드는 걸까?

- [ECMA International](https://ecma-international.org/)
    - 정보와 통신 시스템을 위한 국제적 표준화 기구
    - 1961년 결성
    - 본부: 스위스 제네바
    - 기존에는 European Computer Manufacturers Association 의 약자로 시작 → 현재는 ‘인터내셔널’을 붙여 국제적으로 확장
- [TC39](https://github.com/tc39)
    - ECMA International 에 있는 여러 기술 위원회 중, **TC39** (Technial Committee) 가 명세 관리


### 흥미로운…

2024.05.07  
한국 KAIST 연구 ↔ Ecma TC39

> 인간 친화적인 형태인 **영어로 작성한 자연어 명세**에서 컴퓨터에 친화적인 형태인 **기계화 명세를 자동으로 추출**해 이를 기반으로 자바스크립트 생태계 안정성을 보장하는 기술을 개발하는데 성공했다
…
이러한 장점을 인정받아, 자바스크립트 언어의 명세를 관리하는 위원회에서는 자바스크립트에 새로운 기능을 추가할 때마다 이 기술을 **필수적으로 사용**하도록 했다. 이 기술은 **자바스크립트 언어의 명세를 작성하는 도중에도 결함을 검출**할 수 있어서, 자바스크립트 언어의 설계 초기 단계에서 발생할 수 있는 결함을 줄이는 효과를 보였다.
>

[ecma-international.org](https://ecma-international.org/news/ecma-international-enables-innovative-research-between-the-korean-kaist-research-group-and-tc39-ecmascript/)  
[NEWS](https://www.kaist.ac.kr/news/html/news/?mode=V&mng_no=36610)  
[관련 유튜브](https://www.youtube.com/watch?v=JGxc-KIUnQY)  
[cacm.acm.org](https://cacm.acm.org/research/javascript-language-design-and-implementation-in-tandem/)

## 버전

| 판 | 출판일 | 이름 | 이전 판과의 차이점 |
| --- | --- | --- | --- |
| 1 | 1997년 6월 |  | **초판** |
| 2 | 1998년 6월 |  | ISO/IEC 16262 국제 표준과 완전히 동일한 규격을 적용하기 위한 변경. |
| 3 | 1999년 12월 |  | 강력한 정규 표현식, 향상된 문자열 처리, 새로운 제어문 , **try/catch** 예외 처리, 엄격한 오류 정의, 수치형 출력의 포매팅 등. |
| 4 | 버려짐 |  | 4번째 판은 언어에 얽힌 **정치적 차이**로 인해 버려졌다. 이 판을 작업 가운데 일부는 5번째 판을 이루는 기본이 되고 다른 일부는 ECMA스크립트의 기본을 이루고 있다. |
| 5 | 2009년 12월 |  | 더 철저한 오류 검사를 제공하고 오류 경향이 있는 구조를 피하는 하부집합인 "**strict mod**e"를 추가한다. 3번째 판의 규격에 있던 수많은 애매한 부분을 명확히 한다.[[3]](https://ko.wikipedia.org/wiki/ECMA%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8#cite_note-3) |
| 5.1 | 2011년 6월 |  | ECMA스크립트 표준의 제 5.1판은 ISO/IEC 16262:2011 국제 표준 제3판과 함께 한다. |
| **6** | **2015년 6월** | **ECMAScript 2015 (ES2015)** | 6판에는 클래스와 모듈 같은 복잡한 응용 프로그램을 작성하기 위한 새로운 문법이 추가되었다. 하지만 이러한 문법의 의미는 5판의 strict mode와 같은 방법으로 정의된다. 이 판은 "ECMAScript Harmony" 혹은 "ES6 Harmony" 등으로 불리기도 한다. |
| 7 | 2016년 6월 | ECMAScript 2016 (ES2016) | 제곱연산자 추가, Array.prototype**.includes** |
| 8 | 2017년 6월 | ECMAScript 2017 (ES2017) | 함수 표현식의 인자에서 trailing commas 허용, Object **values/entries** 메소드, **async/await** 등. |
| 9 | 2018년 6월 | ECMAScript 2018 (ES2018) | **Promise.finally,** Async iteration, object rest/spread property 등. |
| 10 | 2019년 6월 | ECMAScript 2019 (ES2019) | Object.**fromEntries**, flat, flatMap, Symbol.description, optional catch 등. |

cf) https://ko.wikipedia.org/wiki/ECMA%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8

## 어떻게 생겨나는 걸까?

- Stage 0: 허수아비 (strawman)
    - TC39의 컨트리뷰터로 등록한 누구라도 제안 가능
    - 회의의 안건으로 상정되고 앞서 언급된 0단계 문서에 등재되면 0단계 제안
- Stage 1: 제안
    - 챔피언(champion) 을 구해야 한다
        - 챔피언이란 해당 제안을 책임지고 다음 단계로 끌고 나아갈 TC39 구성원
    - 풀고자 하는 문제와 하이 레벨 API 및 잠재적 장애물을 제시
    - 구현상으로는 폴리필, 데모 등을 필요
- Stage 2: 초고
    - ECMAScript 표준의 형식 언어(formal description)로 작성 된 형식적인 서술(formal description) 초안이 필요
    - 실제로 표준에 편입 될 경우 사용할 명세의 초기 버전
    - 2.7….
- Stage 3: 후보
    - 대부분 완성에 가까우며, 구현 주체나 사용자들로부터 피드백을 좀 더 받아보는 일만이 남은 상태
    - 2단계의 초안과는 다르게 모든 부분이 기술되어 있도록 마무리 된 명세가 필요
- Stage 4: 완료
    - 제안이 수락되고 다음 표준에 포함되어 발표되기만을 기다리는 단계
    - ECMA-262의 단위 테스트 슈트인 Test262에 관련 테스트가 작성 + 최소 2개 이상의 구현이 제공되는 등의 까다로운 추가 조건을 모두 만족 → 4단계 승급
    - 별다른 이변이 없는 이상 다가오는 새 표준에 포함되어 발표

[notes/meetings at main · tc39/notes](https://github.com/tc39/notes/tree/main/meetings)

## **ECMAScript 2024 !?**

![img.png](images/img.png)

### `groupBy` 메서드

- **데이터 그룹화**하는 데에 유용
- Object.groupBy, Map.groupBy
- 특정 속성을 기준으로, 데이터를 그룹별로 분할 가능
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Object/groupBy

```jsx
Object.groupBy(items, callbackFn)
```

- items: 이터러블한 그룹화될 데이터
- callbackFn: (element, index) ⇒ { key }
    - 그룹 key 반환 필수

// 예제

```jsx
// Create an Array
const fruits = [
  {name:"apples", quantity:300},
  {name:"bananas", quantity:500},
  {name:"oranges", quantity:200},
  {name:"kiwi", quantity:150}
];

// Callback function to Group Elements
function myCallback({ quantity }) {
  return quantity > 200 ? "ok" : "low";
}

// Group by Quantity
const result = Object.groupBy(fruits, myCallback);

// 결과
{
    "ok": [
        {
            "name": "apples",
            "quantity": 300
        },
        {
            "name": "bananas",
            "quantity": 500
        }
    ],
    "low": [
        {
            "name": "oranges",
            "quantity": 200
        },
        {
            "name": "kiwi",
            "quantity": 150
        }
    ]
}
```

### `Promise.withResolvers` 메서드

- 프로미스를 생성하기 위한 새로운 방법
- 자바스크립트는 Promise를 취소하는 API를 제공X -> 결과를 무시하는 방법 추가
- https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise/withResolvers

```jsx
// AS-IS : new Promise
let resolve, reject;
const promise = new Promise((res, rej) => {
  resolve = res;
  reject = rej;
});

// TO-BE : Promise.withResolvers
const { promise, resolve, reject } = Promise.withResolver();
```

// 예제  
비동기 함수를 전달받아 실행과 취소를 할 수 있는 함수를 만들 수 있습니다.

```jsx
const buildCancelablePromise = <T>(asyncFn: () => Promise<T>) => {
  let rejected = false;

  const { promise, resolve, reject } = Promise.withResolvers<T>();

  return {
    run: () => {
      if (!rejected) {
        asyncFn().then(resolve, reject);
      }

      return promise;
    },

    cancel: () => {
      rejected = true;
      reject(new Error("CanceledError"));
    },
  };
};
```

콜백으로 전달한 비동기 함수는 실행되었으나 완료되기 전 미리 `cancel`를 통해 취소할 수 있습니다.

```jsx
const newPromise = buildCancelablePromise(async () => {
  await sleep(2000);
  return "resolve";
});

(async () => {
  try {
    const value = await newPromise.run();
    console.log("value: ", value);
  } catch (err) {
    console.log("err: ", err);
  }
})();

setTimeout(() => {
  // 프로미스 작업이 완료되기 전 cancel
  newPromise.cancel();
}, 500);
```