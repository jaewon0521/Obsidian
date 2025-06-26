#### 개요
스트리밍은 서버에서 전체 페이지 HTML을 완성한 후 일괄 전송하는 대신, 준비된 렌더링 결과부터 **점진적으로 HTML을 클라이언트에 스트림 형태로 전송**하는 기술이다. 이 방식은 React 18의 Server-Side Rendering(SSR) 개선 기능과 Suspense를 기반으로 하며, 초기 로딩 속도를 크게 향상시킨다.

#### 사용법
##### 페이지 스트리밍
Next.js App Router에서 페이지나 레이아웃 컴포넌트 내부에 `loading.tsx` 파일을 정의하면 해당 페이지는 자동으로 스트리밍 처리된다. `loading.tsx`는 Suspense fallback 역할을 하며, 주요 콘텐츠가 준비되는 동안 사용자에게 로딩 UI를 제공한다.

- 페이지가 `async`로 정의되어 있으면, `await`를 포함한 비동기 작업을 통해 Suspense와 함께 더욱 명확한 스트리밍 타이밍을 조정할 수 있다.
- 상위 레이아웃의 `loading.tsx`는 하위 페이지/레이아웃에도 자동으로 적용된다.

###### 주의사항
- `searchParams`(query string)가 변경되어도 자동으로 새 데이터를 스트리밍하지는 않는다. 이 경우 클라이언트 사이드에서 `key`를 변경해 강제 Suspense 재렌더링을 유도해야 한다.
- 페이지 이동이 아닌 클라이언트 상태 변경만으로는 새로운 서버 요청이 발생하지 않을 수 있다.

##### 컴포넌트 스트리밍
`<Suspense>` 컴포넌트로 감싼 Server Component나 Client Component에서 **지연되는 컴포넌트를 fallback UI로 대체하고**, 준비가 완료되면 자동으로 교체된다. 이 방식은 세부적인 컴포넌트 단위로도 스트리밍 효과를 구현할 수 있게 해준다.

예를 들어 `searchParams`가 변경될 때 `<Suspense key={query}>`로 강제 리렌더링하면, 새로운 데이터를 서버에서 받아오는 동안 fallback UI를 보여줄 수 있다.
