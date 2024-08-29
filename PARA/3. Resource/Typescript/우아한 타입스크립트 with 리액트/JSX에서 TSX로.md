#### React.ReactElement
JSX의 createElement 메서드 호출로 생성된 리액트 엘리먼트를 나타내는 타입이다.
```ts
interface ReactElement<P = any, T extemds string | JSXElementConstructor<any> = string | JSXElementConstructor<any>>{
	type: T;
	props: P;
	key: Key | null;
}
```

#### JSX.Element
```ts
interface Element extends React.ReactElement<any, any> { }
```

#### React.ReactNode
ReactNode는 ReactElement 타입을 포함하고 있어 좀 더 넓은 범위를 갖고 있다.
리액트의 render 함수가 반환할 수 있는 모든 형태를 담고 있다고 볼 수 있다.
```ts
type ReactText = string | number;
type ReactChild = ReactElement | ReactText;
type ReactFragment = {} | Iterable<ReactNode>;

type ReactNode = ReactChild | ReactFragment | ReactPortal | boolean | null | undefined;

type COmponentType<P = {}> = ComponentClass<P> | FunctionComponent<P>; 
```

세가지 타입의 관계를 정리하면 다음과 같다.
ReactNode > ReactElement > JSX.Element 