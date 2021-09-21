# javascript-reverse-tracing

## Goal

 - 요청의 특정 파라미터들이 어떻게 만들어지는가?
 - 변수 캡쳐링

## Static Analysis

### Function Analysis

함수에는 입력과 출력이 있다.
입력과 출력 사이의 상관관계를 분석해보자

#### 변수의 생존 범위

함수내에서 `var`로 선언된 변수는 함수 전체에서 유효하다.
함수내에서 `let`로 선언되 변수는 블럭 안쪽에서만 유효하다.
함수 밖에 선언된 변수는 모든 곳(전역)에서 유효하다.

다음 예제를 보자.

```js
function a() {
  b = 99;
}
a()
console.log(b)
```

```
99
```

`b`는 어디에도 존재하지 않기 때문에 `Implied Globals`로 선언된다.

다음 예제를 보자.

```js
var x = 1;

function foo() {
  var x = 10;
  bar();
}

function bar() {
  console.log(x);
}

foo();
bar();
```

```
1
1
```

`Lexical Scope`는 함수가 어디에서 호출되는지가 아닌 어디에 선언되어있는가에 따라 결정된다.
