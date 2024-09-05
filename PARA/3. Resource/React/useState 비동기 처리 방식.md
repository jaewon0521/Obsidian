`useState`는 **비동기적으로 동작하는 것처럼 보이는 것**에 가깝다. `useState` 자체가 비동기 함수는 아니지만, 상태 업데이트의 결과가 **비동기적으로 처리**되기 때문에 그렇게 느껴질 수 있습니다. React가 상태 업데이트를 batching(일괄 처리)하기 때문이다.
### 이유

1. **상태 업데이트의 예약**: `useState`의 상태 업데이트 함수(`setState`)를 호출하면, 새로운 상태로 변경하는 작업이 **즉시 실행되는 것이 아니라 예약**됩니다. React는 상태 업데이트가 예약되면 이를 스케줄에 따라 처리하며, 그 후 리렌더링을 수행합니다.
    
2. **Batching (일괄 처리)**: React는 성능 최적화를 위해 여러 개의 상태 업데이트를 하나로 묶어서 처리합니다. 예를 들어, 하나의 이벤트 핸들러 내에서 `setState`를 여러 번 호출하면 React는 각 `setState` 호출마다 즉시 렌더링을 하지 않고, 하나의 렌더링으로 통합하여 처리합니다. 이렇게 하면 렌더링을 여러 번 하는 것을 방지하여 성능이 향상됩니다.
    
3. **비동기와 유사한 동작**: 상태 업데이트는 직후에 상태가 변경된 결과를 즉시 반환하지 않으므로, **비동기처럼 보이는** 것입니다. 다음 줄에서 `setState`로 인해 변경된 상태 값을 바로 확인할 수 없기 때문에 비동기처럼 보이는 것이지, `useState` 자체가 비동기 처리를 위한 프로미스(Promise)와 같은 형태로 작동하는 것은 아닙니다.

```tsx
import React, { useState } from 'react'; 

function Counter() { 
	const [count, setCount] = useState(0); 
	const handleClick = () => { 
		setCount(count + 1); console.log(count); // 여전히 이전 상태 값 출력 
	}; 
	
	return ( 
			<div> 
				<p>Count: {count} </p> 
				<button onClick={handleClick}>Increment</button> 
			</div> 
	); 
}
```
위 코드에서 버튼 클릭 시 `setCount(count + 1)`을 호출해도, `console.log(count)`는 **이전 상태 값**을 출력합니다. 이는 `setCount`가 상태를 **즉시** 업데이트하는 것이 아니라 **React의 렌더링 주기**가 끝난 후에 새로운 상태로 업데이트되기 때문입니다.
### 결론
따라서 `useState`는 상태 업데이트의 일괄 처리(batch update)와 스케줄링 때문에 **비동기처럼 동작**하는 것이지, 실제로 비동기 함수는 아닙니다. 