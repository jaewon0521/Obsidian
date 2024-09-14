컴포넌트가 리렌더링되더라도 값이 유지되길 원하거나 DOM의 직접적인 제어가 필요할 때 사용된다.

#### 사용법
```tsx
const ref = useRef(initialValue);
```
- `useRef`는 `current`라는 프로퍼티를 가진 객체를 반환합니다
- `current` 프로퍼티에 저장된 값은 컴포넌트가 **리렌더링되더라도** 그대로 유지되며 변경 시 리렌더링을 일으키지 않는다. 
- 이것이 `useRef`와 `useState`의 큰 차이점입니다.



#### 주의점
**렌더링 중에는 `ref.current`를 쓰거나 _읽지_ 마세요.**

React는 컴포넌트의 본문이 [순수 함수처럼 동작하기](https://ko.react.dev/learn/keeping-components-pure)를 기대합니다:

- 입력값들([props](https://ko.react.dev/learn/passing-props-to-a-component), [state](https://ko.react.dev/learn/state-a-components-memory), [context](https://ko.react.dev/learn/passing-data-deeply-with-context))이 동일하면 완전히 동일한 JSX를 반환해야 합니다.
- 다른 순서나 다른 인수를 사용하여 호출해도 다른 호출의 결과에 영향을 미치지 않아야 합니다.

**렌더링 중에** ref를 읽거나 쓰면 이러한 기대가 깨집니다.

```tsx
function MyComponent() { 
	// ...  
	// 🚩 Don't write a ref during rendering  
	myRef.current = 123;  
	// ...  
	// 🚩 Don't read a ref during rendering  
	return <h1>{myOtherRef.current}</h1>;
}
```

**대신 이벤트 핸들러나 Effect에서** ref를 읽거나 쓸 수 있습니다.

```tsx
function MyComponent() {  
	// ...  
	useEffect(() => {
	// ✅ You can read or write refs in effects    
		myRef.current = 123;  
	});  
	// ...  
	function handleClick() {
		// ✅ You can read or write refs in event handlers 
		doSomething(myOtherRef.current);  
	}  
	// ...
}
```

렌더링 중에 무언가를 읽거나 [써야](https://ko.react.dev/reference/react/useState#storing-information-from-previous-renders)_만_ 하는 경우, 대신 [state를 사용](https://ko.react.dev/reference/react/useState)하세요.

컴포넌트는 이러한 규칙을 어기더라도 여전히 작동할 수도 있지만, React에 추가되는 대부분의 새로운 기능들은 이러한 기대에 의존합니다. 자세한 내용은 [컴포넌트를 순수하게 유지하기](https://ko.react.dev/learn/keeping-components-pure#where-you-_can_-cause-side-effects)에서 확인하세요.