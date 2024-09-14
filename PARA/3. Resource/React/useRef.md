컴포넌트가 리렌더링되더라도 값이 유지되길 원하거나 DOM의 직접적인 제어가 필요할 때 사용된다.

#### 사용법
```tsx
const ref = useRef(initialValue);
```
- `useRef`는 `current`라는 프로퍼티를 가진 객체를 반환합니다
- `current` 프로퍼티에 저장된 값은 컴포넌트가 **리렌더링되더라도** 그대로 유지되며 변경 시 리렌더링을 일으키지 않는다. 
- 이것이 `useRef`와 `useState`의 큰 차이점입니다.

