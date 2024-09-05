#### 개요
useState는 React의 주요 훅(Hook) 중 하나로, 함수형 컴포넌트에서 상태를 관리하기 위해 사용된다.

#### useState 목적
React는 초기에는 클래스형 컴포넌트를 사용하여 상태(state)를 관리했다.
클래스형 컴포넌트는 상태 관리와 생명주기 메서드(Lifecycle Methods)를 포함할 수 있었지만, 이를 작성하고 유지 관리하는 것은 복잡하다. 

React 팀은 이를 개선하기 위해 **Hooks**라는 개념을 도입했고, 그 중 하나가 `useState`이다. 
`useState`는 함수형 컴포넌트에서 **상태 관리**를 할 수 있도록 해준다. 
이를 통해 코드의 가독성과 재사용성을 높일 수 있으며, 클래스형 컴포넌트를 사용할 때 발생하는 복잡성을 줄일 수 있다.

#### [[useState 비동기 처리 방식]]
`useState`는 **비동기**적으로 상태를 업데이트한다. 
React에서는 성능 최적화를 위해 여러 상태 업데이트를 하나로 병합하거나, 배치(batch)하여 처리하는 방식을 사용한다. 이는 상태가 업데이트될 때마다 즉각적으로 컴포넌트를 다시 렌더링하는 것이 아니라, 필요할 때 한꺼번에 업데이트를 처리하여 성능을 개선하기 위함이다.

- 상태 업데이트 함수(예: `setState`)는 호출된 직후 바로 상태를 업데이트하지 않는다.
- React는 이벤트 핸들러나 생명주기 메서드가 종료된 후에 한꺼번에 상태 업데이트를 적용합니다. 이를 **batching**이라고 부른다.
- 이로 인해, 상태를 연속적으로 업데이트하는 경우에도 각 상태가 갱신되기 전의 상태 값을 참조하게 될 수 있다.

#### useState 동작 원리
`useState`의 동작 원리는 React의 상태 관리와 가상 DOM(Virtual DOM) 개념을 이해하는 데 있다.

- **상태 저장**: `useState`는 React의 내부 메모리에서 현재 컴포넌트의 상태를 저장합니다. 컴포넌트가 마운트될 때 초기화되고, 이후 상태가 변경되면 내부적으로 업데이트됩니다.
- **상태 변경 요청**: `setState`가 호출되면 React는 현재 상태와 비교하여 새로운 상태로 변경할지를 결정합니다.
- **리렌더링**: 상태가 변경되면 React는 해당 컴포넌트를 다시 렌더링합니다. 이때 React는 변경된 부분만 실제 DOM에 반영하여 효율적으로 업데이트를 수행합니다.
- **배치 업데이트**: 성능 최적화를 위해 상태 업데이트는 비동기적으로 처리됩니다. 여러 개의 상태 업데이트가 발생하면, React는 이를 하나의 업데이트로 병합하여 처리합니다.

#### useState 초기값 지연 평가(Lazy Initialization)
`useState`의 초기값은 함수로 전달할 수 있습니다. 이렇게 하면 초기값 계산이 필요할 때만 한 번 실행되며, 초기 렌더링 성능을 최적화할 수 있습니다.


```tsx
import React, { useState } from 'react';  
function ExpensiveComponent() {   
// 초기값을 함수로 전달하여 무거운 연산을 처음 한 번만 실행   
	const [value, setValue] = useState(() => {     
		console.log('Computing initial value...');     
		return expensiveComputation(); // 무거운 연산   
	});    
	
	return (     
		<div>       
			<p>Value: {value}</p>
		    <button onClick={() => setValue(value + 1)}>Increment</button>  
	    </div>   
	); 
}  

function expensiveComputation() {   
	// 가상으로 무거운 연산 수행   
	let sum = 0;   
	
	for (let i = 0; i < 1000000000; i++) {     
		sum += i;   
	}   
	
	return sum; 
}
```

여기서 `expensiveComputation` 함수는 컴포넌트가 처음 렌더링될 때 한 번만 실행됩니다. 만약 함수 없이 초기값을 전달하면 컴포넌트가 렌더링될 때마다 계산이 실행될 수 있습니다.