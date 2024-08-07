리액트가 이벤트를 어떻게 관리 할까

![](https://miro.medium.com/v2/resize:fit:1400/0*etYCw6ApSzaoScLo.png)

리액트 공식문서에 따르면, React 17버전 부터 이벤트 핸들러를 "Document" 레벨이 아닌 리액트 트리가 렌더되는 root DOM Container에 attach 합니다.
즉, 이벤트 핸드러를 Button, Input 등 다른 컴포넌트에 붙이든 상관없이 모든 이벤트 핸들러들이 root DOM Container에 부착된다는 의미 입니다.
다음과 같이 실제로 Native Event Handler가 button node가 아닌 root node에 attach된 것을 확인 할 수 있습니다.

```
import { MouseEvent, useCallback } from "react";
  
function App() {
	const handleClick = useCallback((e: MouseEvent<HTMLButtonElement>) => {
	
	console.log(e.nativeEvent.currentTarget);
}, []);

return (
		<div>
			<button onClick={handleClick}>hello</button>
		</div>
	);
}
```

![](https://miro.medium.com/v2/resize:fit:528/1*Mgf47IV2JwYrWTZMk4tE2g.png)

#### Synthetic Event
리액트는 브라우저 호환(크로스 브라우징)을 위해 NativeEvent를 그대로 사용하는 것이 아닌 SyntheticEvent 객체를 이용해서 NativeEvent를 감싸는 방식을 사용 합니다.
BaseSyntheticEvent 타입을 통해 알 수 있듯, 브라우저 NativeEvent를 비롯해, 해당 이벤트를 발생시킨 FiberNode의 정보등을 가지고 있습니다.
실제로 이벤트가 발생했을 때 이벤트 핸들러에 전달하는 값은 NativeEvent가 아닌 바로 SyntheticEvent 객체가 됩니다.

```
type BaseSyntheticEvent = {
	isPersistent: () => boolean,
	isPropagationStopped: () => boolean,
	_dispatchInstances?: null | Array<Fiber | null> | Fiber,
	_dispatchListeners?: null | Array<Function> | Function,
	_targetInst: Fiber,
	nativeEvent: Event,
	target?: mixed,
	relatedTarget?: mixed,
	type: string,
	currentTarget: null | EventTarget,
};
```

해당 SyntheticEvent를 사용하기 때문에 리액트에서는 이 이벤트를 처리하는 핸들러를 따로 보유하게 되는 것이며, NativeEvent와 이 이벤트 핸들러를 매핑해주는 단계가 필요합니다.

리액트가 처리하는 이벤트 핸들러를 root DOM Node에 붙이는 과정은 Virtual DOM(Fiber Tree)를 생성하는 시점에 일어납니다.
리액트는 이 Fiber Tree를 root DOM Node에 *reactRootContainer* 라는 key로 저장하게 되며, 리액트에서 처리하는 이벤트 핸들러들은 이 Fiber Tree가 생성되고 나면, 단계를 거쳐 root DOM Node에 attach 됩니다.

##### STEP 1 :

> [!NOTE]
> 'click', 'change' ,'dblclick' 등 핸들링 되어야 할 모든 **Native Event들의 목록을 정리**합니다.

리액트는 코드 안에 NativeEvent List를 저장해두고 있습니다.

```
const discreteEventPairsForSimpleEventPlugin = [
	('cancel': DOMEventName), 'cancel',
	('click': DOMEventName), 'click',
	('close': DOMEventName), 'close',
	('contextmenu': DOMEventName), 'contextMenu',
	('copy': DOMEventName), 'copy',
	('cut': DOMEventName), 'cut',
	('auxclick': DOMEventName), 'auxClick',
	('dblclick': DOMEventName), 'doubleClick', // Careful!
	('dragend': DOMEventName), 'dragEnd',
	('dragstart': DOMEventName), 'dragStart',
	... some other events
];
```

##### STEP 2:
NativeEvent 이름과 리액트 이벤트 핸들러 Property를 매핑합니다.
이를테면 click: 'onClick', change:'onChange'와 같은 형식입니다. 이렇게 생성된 매핑 테이블은 추후 이벤트가 발생했을 때, 이를 적절한 이벤트 핸들러와 연결해 줍니다.

##### STEP 3:
**Discrete Event, UserBlocking Event, Continuous Event** 등 리액트에서 정의한 이벤트 타입에 따라 부여하는 이벤트의 우선순위가 다른데, 전체 Native Event를 리**액트에서 부여하는 기준에 맞게 우선순위를 설정**합니다.

##### STEP 4:
Virtual DOM(root Fiber Node)에 이벤트 핸들러들을 등록하는 과정을 거칩니다.
