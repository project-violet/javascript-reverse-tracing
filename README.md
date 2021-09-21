# javascript-reverse-tracing

## Goal

 - 요청의 특정 파라미터들이 어떻게 만들어지는가?
 - 변수 캡쳐링

## Static Analysis

### Scope

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

### Function Analysis

#### Live Variable Analysis (Liveness Analysis)

함수에는 입력과 출력이 있다.
입력이나 출력에 의존하지 않고 Scope에 의존하기도 한다.
`IN` 집합은 가능한 모든 입력들이다.
`OUT` 집합은 가능한 모든 출력들이다.

```js
a=4
function b(c,d,e) {
c+=1
if (d>0) return e;
return d
}
b(1,2,3)
```

`b`함수의 `IN`은 `IN={a,c,d,e}`이다. 
`a`는 전역에 선언된 변수이며, `c,d,e`는 `b`의 인자들이다.
따라서 `a,c,d,e`는 가능한 모든 입력들이다.
