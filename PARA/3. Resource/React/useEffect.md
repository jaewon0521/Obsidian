클래스 컴포넌트의 componentDidUpdate, componentDidMount, componentWillUnmount 생명주기 메서드의 기능을 제공한다.

#### 동작 순서
1. **컴포넌트가 처음 렌더링되고 DOM이 준비된 후 `useEffect` 콜백 실행**
2. **디펜던시 변경 시 실행**
3. **언마운트 시 또는 다음 이펙트 실행 전 정리**


#### Clean Up 함수
```tsx
	useEffect(() => {
		const handle = setInterval(() => {
			console.log("## tick!);
		},1000)

		// cleanUp 함수
		return () => {
			clearInterval(handle);
		}
	}, [])
```

- effectCallback 함수 내부에서 클린업 함수를 리턴하도록 작성 할 수 있다.
- 클린업 함수는 주로 메모리 누수를 방지하고, 타이머, 구독 등 불필요한 리소스를 해제하는 데 사용된다.

#### Clean Up 함수 동작 순서
1. **컴포넌트 마운트** → `useEffect` 실행 (클린업 함수 없음)
2. **컴포넌트 재렌더링** (의존성 값 변경) → 기존 클린업 함수 실행 → 새로운 `useEffect` 실행
3. **컴포넌트 언마운트** → 클린업 함수 실행