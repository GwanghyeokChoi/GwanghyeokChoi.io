---
title: 실행 컨텍스트
date: "2022-09-05T22:40:32.169Z"
description: 호이스팅 개념에 추가로 붙는 설명
---

# 개념

---

```jsx
실행 컨텍스트
- 실행할 코드에 제공할 환경 정보들을 모아놓은 객체
- 자바스크립트의 동적 언어로서의 성격을 가장 잘 파악할 수 있는 개념
```

자바스크립트는 실행 컨텍스트가 활성화되는 시점에 다음과 같은 현상이 발생한다.

- 호이스팅이 발생한다 (선언된 변수를 위로 끌어올린다)
- 외부 환경 정보를 구성한다
- this 값을 설정한다.

이로 인해 다른 언어에서 발견할 수 없는 특이한 현상들이 발생한다.

# 실행 컨텍스트 구성

---

실행 컨텍스트는 다음과 같은 것들을 이용하면 `call stack` 에 쌓이게 된다.

- `전역공간` 은 자동으로 컨텍스트로 구성된다.
- `함수`를 실행한다.
- `eval()` 함수를 실행한다.
- `block`을 만든다 (ES6+)

일반적으로 함수를 이용한 실행 컨텍스트를 사용한다.

```jsx
var a = 1; // 전역 컨텍스트
function outer() { // outer 컨텍스트
	function inner () { // inner 컨텍스트
		console.log(a); // undefinde
		var a = 3;
		console.llog(a); // 3
	}
	inner();

	console.log(a); //1
}
outer();
console.log(a);
```

위와 같이 코드를 구성했을 때 실행 컨텍스트의 스택은 다음과 같은 순서로 실행된다.

- 프로그램 실행: `[전역컨텍스트]`
- outer 실행: `[전역컨텍스트, outer]`
- inner 실행: `[전역컨텍스트, outer, inner]`
- inner 종료: `[전역컨텍스트, outer]`
- outer 종료: `[전역컨텍스트]`

그리고 이러한 실행컨텍스트를 구성할 때 생기는 것들이 있다.

- `VariableEnvironment`
    - 현재 컨텍스트 내의 식별자(변수)들에 대한 정보
    - 외부 환경 정보
    - 선언 시점의 LexicalEnvaironment의 스냅샷(변경사항 반영 X)
- `LexicalEnvironment`
    - 처음에는 VariableEnvironment와 같음
    - 변경 사항이 실시간으로 반영됨
- `ThisBinding`
    - 식별자가 바라봐야 할 대상 객체

## Variable Environment

VariableEnvironment에 담기는 내용은 LexicalEnvironment와 같지만, 최초 실행 시의 스냅샷을 유지한다.

실행 컨텍스트를 생성할 때 VariableEnvironment에 대한 정보를 먼저 담은 다음, 이를 복사하여 LeicalEnvironment를 만든다.

주로 활용하는 것은 LexicalEnvironment이다. 즉, VariableEnviroment는 스냅샷 유지를 목적으로 사용한다.

## Lexcial Environment

LexicalEnvironment의 내부에서는 environmentRecord와 outerEnvironmentReference로 구성 되어 있다

- environmentRecord로 인하여 호이스팅이 발생한다.
- outerEnvironmentReference로 인하여 스코프와 스코프체인이 형성된다.