#### React.ReactElement
JSX의 createElement 메서드 호출로 생성된 리액트 엘리먼트를 나타내는 타입이다.
```ts
interface ReactElement<P = any, T extemds string | JSXElementConstructor<any> = string | JSXElementConstructor<any>>{
	type: T;
	props: P;
	key: Key | null;
}
```
props를 ReactElement의 제네릭으로 지정해 주어야 한다면. JSX.Element 대신에 ReactElement를 사용하여 추론 관점에서 더 유용하게 사용할 수 있다.

```tsx
const Item = ({ icon } : Props ) => {
	// icon propㅇ
	const iconSize = icon.props.size;
}
```

#### JSX.Element
```ts
interface Element extends React.ReactElement<any, any> { }
```
JSX.Element는 위에 언급한 대로 props와 타입 필드가 any 타입인 ReactElement를 나타낸다. 엘리먼트를 prop으로 전달받아 render props 패턴으로 컴포넌트를 구현할 때 유용하게 활용할 수 있다.

```tsx
interface Props {
	icon: JSX.Element;
}

const Item = ({ icon } : Props) => {
	// prop으로 받은 컴포넌트의 props에 접근할 수 있다.
	const iconSize = icon.props.size;
	
	return <li>{icon}</li>
}

const App = () => {

	return <Item icon={<Icon size={14} />} />
}
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