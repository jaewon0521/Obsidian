기존 리액트 앱에서는 모든 JS 번들을 다운로드 한 후 컴포넌트가 마운트 되었을 떄 백엔드 서버에게 데이터를 요청하기 때문에 사용자에게 컨텐츠를 제공하는 시간이 길어질 수 밖에 없는 단점이 있습니다.

이런 불편함을 Next JS에서는 사전 렌더링 방식에서 데이터 페칭을 포함하여 더 빠르게 사용자에게 컨텐츠를 제공 할 수 있습니다.

- 기본적인 사전 렌더링 방식으로 요청이 들어올 때 마다 사전 렌더링을 진행 한다.(SSR)
- RES 데이터 사이즈가 너무 크거나 서버의 상태가 좋지 않을 경우 Build Time시점에 데이터 페칭을 할 수 있다. `[요청이 오래 걸릴 것 같은 페이지]` (SSG)
- (ISR)

#### SSR
``` typescript
export const getServerSideProps = async() => {
	const data = await getFetch();
	
	return {
		props : { data }
	}
}

export default function Page(
{ data } : InfetGetServerSidePropsType<typeof getServerSideProps>
) {
	console.log(data);
	
	return ...
}
```

#### SSG
```typescript

export const getStaticProps = async() => {
	const data = await getFetch();
	
	return {
		props : { data }
	}
}

export default function Page(
{ data } : InfetGetStaticPropsType<typeof getStaticProps>
) {
	console.log(data);
	
	return ...
}

```
