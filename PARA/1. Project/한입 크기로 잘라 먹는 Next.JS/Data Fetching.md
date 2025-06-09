기존 리액트 앱에서는 모든 JS 번들을 다운로드 한 후 컴포넌트가 마운트 되었을 떄 백엔드 서버에게 데이터를 요청하기 때문에 사용자에게 컨텐츠를 제공하는 시간이 길어질 수 밖에 없는 단점이 있습니다.

이런 불편함을 Next JS에서는 사전 렌더링 방식에서 데이터 페칭을 포함하여 더 빠르게 사용자에게 컨텐츠를 제공 할 수 있습니다.

- 기본적인 사전 렌더링 방식으로 요청이 들어올 때 마다 사전 렌더링을 진행 한다. (SSR)
- RES 데이터 사이즈가 너무 크거나 서버의 상태가 좋지 않을 경우 Build Time시점에 데이터 페칭을 할 수 있다. `[요청이 오래 걸릴 것 같은 페이지]` (SSG)
- SSG방식으로 생성된 정적 페이지를 일정 시간을 주기로 다시 생성한다. (ISR)

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

#### SSG 동적 경로
/[id] 처럼 경로에 변수가 들어가는 경우에 해당합니다.

``` typescript 
export const getStaticPaths: GetStaticPaths = async () => {
  const res = await fetch(url);
  const datas = await res.json();

  const paths = datas.map((data: Data) => ({
    params: { id: data.id.toString() },
  }));

  return {
    paths,            // ['/path/1', '/path/2', '/path/3' , ...] 빌드 시 생성
    fallback: false,  // fallback이 false면 위 경로 외에는 404
  };
};
```
##### falback
 - fallback 상태는 useRouter의 isFallback으로 확인 할 수 있다. (로딩 바)

| fallback 값   | 의미                                                                                                                            |
| ------------ | ----------------------------------------------------------------------------------------------------------------------------- |
| `false`      | 지정한 경로만 HTML로 생성. 그 외 요청은 404                                                                                                 |
| `true`       | 지정하지 않은 경로는 **요청 시 생성**, 로딩 UI 필요 <br>데이터가 없는 UI 페이지를 빠르게 반환 <br>첫 번째는 SSR로 HTML 파일은 Next Server에 생성 후 두 번째 요청 부터 SSG 방식으로 동작 |
| `'blocking'` | 지정하지 않은 경로도 요청 시 서버에서 생성하고 완성된 HTML을 바로 응답                                                                                    |
|              | 첫 번째는 SSR로 HTML 파일은 Next Server에 생성 후 두 번째 요청 부터 SSG 방식으로 동작                                                                  |

#### ISR
``` typescript 
export const getStaticProps = async() => {
	const data = await getFetch();
	
	return {
		props : { data },
		revaliate : 60 // 60s
	}
}

export default function Page(
{ data } : InfetGetStaticPropsType<typeof getStaticProps>
) {
	console.log(data);
	
	return ...
}

```
