#### RSC 도입 목적
### ❓ 1. Page Router 시절에는
- **데이터 Fetch를 Page Component 단에서만** 할 수 있었고
- 모든 데이터를 상위에서 가져와서 `props`로 자식 컴포넌트에게 전달해야 했기 때문에
- **컴포넌트 분리/재사용성이 떨어졌고**, **페이지 설계가 무거워졌음**
### ❓ 2. App Router에서는
- 각 **Server Component가 자체적으로 async/await + fetch로 데이터를 가져올 수 있어서**, 컴포넌트 단위의 **독립성과 재사용성이 높아졌음**
### ❓ 3. 그런데, **Context API로 이 문제를 해결할 수 있는 것처럼 보이는데도**
- 왜 굳이 Server Component (RSC)를 도입한 것일까?
- 그리고 그게 정말 실질적인 이점이 있는 걸까?
    
➡️ ✅ 아주 좋은 질문이며, 이것이 바로 **React Server Components(RSC)**의 근본적인 존재 이유와 맞닿아 있습니다.

---

## ✅ 답변: Context API로는 해결할 수 없는 RSC의 핵심 가치
### 1. ✅ **RSC의 목적은 ‘데이터 흐름의 단순화’가 아니다.**
**RSC의 본질은 ‘클라이언트 번들 크기 최적화’ + ‘서버 자원 활용’이다.**
#### 예를 들어:
``` tsx
// Server Component
export default async function ProductCard({ id }: { id: string }) {
  const product = await fetchProduct(id); // 서버에서 fetch
  return <div>{product.name}</div>;
}
```

- 이 컴포넌트는 **서버에서 실행되므로** `fetchProduct()`가 **클라이언트 번들에 포함되지 않음**
- `product.name`만 HTML로 렌더링되어 클라이언트로 전달됨
- 👉 **데이터도 클라이언트에 가지 않고, fetch 로직도 번들에 포함되지 않음**
    
---

### 2. ❌ **Context API는 클라이언트에서 동작한다**
- Context API는 결국 **브라우저에서 실행**되므로, **초기 렌더링 전에 데이터를 미리 준비**해놓아야 함
- 즉, 여전히 Page Component 또는 Layout에서 fetch하고 Context에 주입하는 방식 필요

``` tsx
// page.tsx
const data = await fetchSomething();
return (
  <MyContext.Provider value={data}>
    <SubComponent />
  </MyContext.Provider>
);
```
- 이 구조는 결국 Page Router 시절의 **“상위 → 하위 전달” 구조**와 똑같음
- SSR 시에도 Context의 값을 hydrate 하기 위해 JS 코드가 복잡해지고 **번들 크기 증가** 발생
    
---

### 3. ✅ RSC는 ‘props drilling’ 해소가 아니라 ‘JS 없는 Server 렌더링’을 위한 기술
- RSC는 클라이언트에 **JS 없이도 전체 트리를 구성 가능**
- 특정 부분만 `use client`로 지정해 그 부분만 클라이언트 상호작용
- 결과적으로 **렌더링 성능 + 초기 페이지 크기 + 개발 편의성**을 동시에 추구
    
---
## 🔍 비교: Context API vs RSC

|항목|Context API|React Server Component (RSC)|
|---|---|---|
|실행 위치|클라이언트|서버|
|데이터 fetch 위치|Page or Layout 수준에서 수동 처리|각 컴포넌트 내부에서 async 가능|
|번들 크기|증가|감소 (필요 없는 JS 제거)|
|hydration 필요|✅ 필요함|❌ 필요 없음 (서버에서 HTML만 전송)|
|목적|상태 전달, 전역 공유|렌더링 최적화, 클라이언트 코드 최소화|

---

## ✨ 그래서 요약하자면:

> Context API는 여전히 유효한 상태 관리 수단이지만,  
> **“페이지 컴포넌트에서 모든 데이터를 수집해서 하위에 전달해야 하는 문제”는 근본적으로 해결하지 못합니다.**

> 반면 **App Router + RSC 구조**는 **각 컴포넌트가 서버에서 독립적으로 fetch 가능하고**,  
> **JS 번들에 포함되지 않으며, 클라이언트엔 순수 HTML만 전달되는 구조라서**  
> 성능, 유지보수, 재사용성 측면에서 훨씬 유리한 아키텍처를 제공합니다.

---
## 💬 면접에서 이렇게 말할 수도 있어요

> Page Router 시절에는 페이지 단에서 모든 데이터를 수집하고 자식에 전달해야 했는데,  
> App Router와 RSC의 도입으로 각 컴포넌트가 독립적으로 서버에서 데이터를 fetch할 수 있게 되었습니다.  
> Context API로도 전달 문제는 해결할 수 있지만,  
> Context는 클라이언트 실행 기반이라 여전히 hydration, JS 번들 증가 등의 부담이 있습니다.  
> 반면 RSC는 클라이언트에 JS 없이 HTML만 전달되므로 **성능 최적화와 아키텍처 분리가 명확해지고**,  
> 개발 경험 또한 더 나아졌다고 생각합니다.