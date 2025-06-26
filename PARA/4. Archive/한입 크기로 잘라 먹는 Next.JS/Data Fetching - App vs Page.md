#### 1. App Router (app 디렉토리)
##### 데이터 페칭 방식
- **서버 컴포넌트**에서 직접 `async/await fetch()`로 데이터 요청 가능
- fetch의 옵션(`cache`, `next: { revalidate }`, `tags`)을 통해 SSR/SSG/ISR/On-Demand Revalidate를 구현
- **클라이언트 컴포넌트**에서는 기존처럼 fetch, SWR, React Query 등 사용

##### 주요 fetch 옵션
```typescript
// 캐시 사용 (기본값)
const res = await fetch("/api/data", { cache: "force-cache" });

// 캐시 미사용 (항상 최신 데이터)
const res = await fetch("/api/data", { cache: "no-store" });

// ISR: 3초마다 캐시 갱신
const res = await fetch("/api/data", { next: { revalidate: 3 } });

// On-Demand Revalidate용 태그 지정
const res = await fetch("/api/data", { next: { tags: ["post"] } });
```

##### Request Memoization
- 한 번의 렌더링 동안 동일한 fetch 요청은 한 번만 실행됨(중복 방지)
- 렌더링이 끝나면 캐시는 소멸

##### 특징
- **컴포넌트 단위**로 데이터 페칭 가능
- 서버/클라이언트 컴포넌트가 명확히 분리됨
- getServerSideProps/getStaticProps 등은 사용하지 않음
- 서버 컴포넌트에서만 fetch가 서버에서 실행됨

---

#### 2. Page Router (pages 디렉토리)
##### 데이터 페칭 방식
- 페이지 단위로 getServerSideProps, getStaticProps, getInitialProps 등 내장 함수 사용
- 함수에서 받은 데이터를 props로 컴포넌트에 전달

##### 주요 함수 예시
```typescript
// SSR
export async function getServerSideProps() {
  const data = await fetchData();
  return { props: { data } };
}

// SSG
export async function getStaticProps() {
  const data = await fetchData();
  return { props: { data } };
}

// ISR
export async function getStaticProps() {
  const data = await fetchData();
  return { props: { data }, revalidate: 60 };
}
```

##### 특징
- **페이지 단위**로 데이터 페칭
- 컴포넌트 단위 데이터 페칭이 어려움
- getServerSideProps/getStaticProps 등 함수로 SSR/SSG/ISR 구분
- 클라이언트/서버 코드가 혼합될 수 있음

---

#### 3. App Router vs Page Router 데이터 페칭 비교

| 구분           | Page Router (/pages)                  | App Router (/app)                   |
| -------------- | ------------------------------------- | ----------------------------------- |
| 데이터 페칭    | getServerSideProps, getStaticProps 등 | fetch + 옵션 (cache, revalidate 등) |
| 페칭 위치      | 페이지 단위                           | 컴포넌트 단위                       |
| SSR/SSG/ISR    | 함수로 구분                           | fetch 옵션으로 구분                 |
| 서버/클라 구분 | 혼합(불명확)                          | 명확(서버/클라 컴포넌트 분리)       |
| 번들 크기      | 클라이언트로 많이 전달됨              | 서버에서 처리, 번들 크기 감소       |

---
