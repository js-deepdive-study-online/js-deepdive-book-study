# 20장 - 22장:: 리액트에서의 This

> 📅 스터디 날짜: 2024년 10월 28일

- state
- props
- refs
- 컴포넌트 method
- 생명주기 method

## 클래스형 컴포넌트

문제를 풀어볼까요 ?

```jsx
import React, {Component} from 'react';

class TestThis extends Component {
    constructor(props) {
        super(props);

        console.log('this (TestThis-constructor) = ', this)

        this.thisFunctionBind = this.thisFunctionBind.bind(this) 
    }

    componentDidMount() {
        console.log('this (TestThis-componentDidMount) = ', this) // TestThis
    }

    componentWillUnmount() {
        console.log('this (TestThis-componentWillUnmount) = ', this) // TestThis
    }

    thisArrow = () => {
        console.log('this (TestThis-thisArrow) = ', this)  // TestThis
    }

    thisArrowBind = () => {
        console.log('this (TestThis-thisArrowBind) = ', this)  // TestThis
    }
    
    thisFunctionBind() {
        console.log('this (TestThis-thisFunctionBind) = ', this) // TestThis
    }
    
    thisFunction() {
        console.log('this (TestThis-thisFunction) = ', this) // undefined
    }

    render() {
        console.log('this (TestThis-render) = ', this)  // TestThis

        return (
            <div>
                <button onClick={this.thisArrow}>thisArrow</button><br/>

                <button onClick={this.thisArrowBind.bind(this)}>thisArrowBind</button><br/>

                <button onClick={this.thisFunctionBind}>thisFunctionBind</button><br/>
                
                <button onClick={this.thisFunction}>thisFunction</button><br/>
            </div>
        );
    }
}

export default TestThis;
```

## 알아보기
> 문제 풀기 전까진 확인 금지 ❌

  ### 인스턴스

  `extends` 키워드를 통해, React.Component를 상속받아 모든 메서드와 속성을 사용할 수 있다.

  Class형 컴포넌트 생성시, `React.Component`, `React.PureComponent` 를 상속해야 리액트의 생명주기 메서드, 상태, 메서드 등을 사용할 수 있다.

  ### 생명주기 method

  componentDidMount, componentWillUnmount 와 같은 라이프사이클 메서드

  ⇒ 호출한 해당 컴포넌트를 가리킨다.

  ### 컴포넌트 method

  컴포넌트 메서드는 주의해서 사용해야 한다!

  호출하는 방식에 따라 결과가 달라진다

    - 화살표 함수
        - 상위 스코프의 `this` 를 그대로 사용 = “lexical this”
        - 상위 스코프인 `render()` method의 this인 TestThis 를 바인딩
    - 함수 선언식
        - this가 동적으로 바인딩
        - ⇒ `bind` 를 통해 this를 고정해야 한다!

```jsx
// this 바인딩 하기
        
// => 1) 생성자 함수 안에서 bind 호출
this.thisBindFunction = this.thisFunctionBind.bind(this)

// => 2) 이벤트 핸들러 속성 안에서 bind 호출
<div onClick={this.thisFunctionBind.bind(this)}> />
```

- 둘 중에 최적화된 방법은 ?
  - 1 !!
  - 매번 `render` 가 호출될 때마다, bind 실행되어 새로운 함수가 생성된다 ⇒ 성능 저하 가능성
  - `constructor` 에서 한 번만 바인딩하여, 처음부터 고정된 this로 저장시키는 것이 최적화된 방법


## 함수형 컴포넌트

```jsx
function MyComponent() {
  const [count, setCount] = useState(0);

  const handleClick = () => {
    setCount((prevCount) => prevCount + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increment</button>
    </div>
  );
}
```

함수형 컴포넌트는 특정 인스턴스에 속하지 않고, 함수 스코프에서 관리되기 때문이다.

이에 따라 내부 메서드들 모두 전역 객체로 this 를 가지고 있기 때문에 ⇒ 의미 없다 !