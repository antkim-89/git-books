---
description: Next.js에서 네비게이팅 하는 법
---

# Link and Navigating

Next.js 내에서 라우팅을 할 수 있는 방법은 4가지입니다.

• `<Link>` 컴포넌트를 이용하는 방법

• `useRouter` 훅을 사용하는 방법 (클라이언트)

• `redirect` 기능을 이용하는 방법 (서버)

• `History API`를 이용하는 방법

이 방법들을 어떻게 사용하는지 익혀봅시다.

## `<Link>` Component

`<Link>`의 경우 `prefetching`기능과 라우트 간의 클라이언트 사이드 네비게이팅을 위해 `<a>` 태그로 만들어진 컴포넌트입니다.

Next.js에서 가장 추천하는 라우팅 방식입니다.

{% endcode %}

{% code title="Link 예시" %}

```js
import Link from "next/link";

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>;
}
```

{% endcode %}

## `useRouter` Hook

`useRouter` 훅의 경우 클라이언트 컴포넌트에서 프로그래밍적으로 라우트를 바꾸는 방법입니다.

{% endcode %}

{% code title="useRouter 예시" %}

```js
"use client";

import { useRouter } from "next/navigation";

export default function Page() {
  const router = useRouter();

  return (
    <button type="button" onClick={() => router.push("/dashboard")}>
      Dashboard
    </button>
  );
}
```

{% endcode %}

## `redirect` Function

서버 컴포넌트에선 `redirect` 기능을 활용하여 라우팅을 합니다.

{% endcode %}

{% code title="redirect 예시" %}

```js
import { redirect } from "next/navigation";

async function fetchTeam(id: string) {
  const res = await fetch("https://...");
  if (!res.ok) return undefined;
  return res.json();
}

export default async function Profile({ params }: { params: { id: string } }) {
  const team = await fetchTeam(params.id);
  if (!team) {
    redirect("/login");
  }

  // ...
}
```

{% endcode %}

{% endcode %}

{% code title="good to know" %}

```
- redirect는 307 (Temporary Redirect) 상태를 기본적으론 리턴합니다. 서버 액션을 사용할 때, 보통 POST 요청에 성공 페이지로 리다이렉팅 하기 위한 결과로 사용되는 303 (See Other)을 리턴합니다.

- redirect는 내부적으로 오류를 발생시키므로 try/catch 블록 외부에서 호출해야 합니다.

- redirect는 렌더링 과정 중 클라이언트 컴포넌트에서 호출할 수 있지만, 이벤트 핸들러에서는 사용할 수 없습니다. 대신 useRouter 훅을 사용할 수 있습니다.

- redirect는 절대 URL도 허용하며 외부 링크로 리다이렉트하는 데 사용할 수 있습니다.

- 렌더링 프로세스 이전에 리다이렉트하려면 next.config.js 또는 미들웨어를 사용하세요.
```
{% endcode %}

## History API

Next.js에서는 브라우저의 History API의 `pushState`와 `replaceState` 메서드를 사용하여 브라우저의 히스토리를 스택을 사용하여 리로딩없이 라우팅을 할 수 있습니다.


### `pushState`

브라우저의 히스토리 스택에 새로운 항목을 추가하는 데 사용합니다. 사용자는 이전 상태로 돌아갈 수 있습니다.

{% endcode %}

{% code title="pushState 예시" %}

```js
'use client'
 
import { useSearchParams } from 'next/navigation'
 
export default function SortProducts() {
  const searchParams = useSearchParams()
 
  function updateSorting(sortOrder: string) {
    const params = new URLSearchParams(searchParams.toString())
    params.set('sort', sortOrder)
    window.history.pushState(null, '', `?${params.toString()}`)
  }
 
  return (
    <>
      <button onClick={() => updateSorting('asc')}>Sort Ascending</button>
      <button onClick={() => updateSorting('desc')}>Sort Descending</button>
    </>
  )
}
```

{% endcode %}


### `replaceState`

브라우저의 히스토리 스택의 현재 항목을 새로운 것으로 변경하는 데 사용합니다. 사용자는 이전 상태로 돌아갈 수 없습니다.

{% endcode %}

{% code title="replaceState 예시" %}

```js
'use client'
 
import { usePathname } from 'next/navigation'
 
export function LocaleSwitcher() {
  const pathname = usePathname()
 
  function switchLocale(locale: string) {
    // e.g. '/en/about' or '/fr/contact'
    const newPath = `/${locale}${pathname}`
    window.history.replaceState(null, '', newPath)
  }
 
  return (
    <>
      <button onClick={() => switchLocale('en')}>English</button>
      <button onClick={() => switchLocale('fr')}>French</button>
    </>
  )
}
```

{% endcode %}

## How Routing and Navigation Works

App Router는 라우팅과 내비게이션에 하이브리드 접근 방식을 사용합니다. 서버에서는 애플리케이션 코드가 라우트 세그먼트별로 자동으로 코드 분할됩니다. 그리고 클라이언트에서는 Next.js가 라우트 세그먼트를 미리 가져와서 캐시합니다. 이는 사용자가 새로운 라우트로 이동할 때 브라우저가 페이지를 다시 로드하지 않고, 변경된 라우트 세그먼트만 다시 렌더링된다는 것을 의미합니다. 이로 인해 내비게이션 경험과 성능이 향상됩니다.

### Code Splitting

코드 분할을 통해 애플리케이션 코드를 더 작은 번들로 나눌 수 있어, 브라우저가 이를 다운로드하고 실행할 수 있습니다. 이는 각 요청에 대한 데이터 전송량과 실행 시간을 줄여 성능을 향상시킵니다.
서버 컴포넌트를 사용하면 애플리케이션 코드가 라우트 세그먼트별로 자동으로 코드 분할됩니다. 이는 내비게이션 시 현재 라우트에 필요한 코드만 로드된다는 것을 의미합니다.

### Prefetching

프리페칭은 사용자가 방문하기 전에 백그라운드에서 라우트를 미리 로드하는 방법입니다.

Next.js에서 라우트를 프리페치하는 두 가지 방법이 있습니다:

1. `<Link>` 컴포넌트: 라우트는 사용자의 뷰포트에 보이게 되면 자동으로 프리페치됩니다. 프리페칭은 페이지가 처음 로드될 때나 스크롤을 통해 뷰포트에 들어올 때 발생합니다.

2. `router.prefetch()`: `useRouter` 훅을 사용하여 프로그래밍 방식으로 라우트를 프리페치할 수 있습니다.

`<Link>`의 기본 프리페칭 동작(즉, `prefetch` prop이 지정되지 않거나 `null`로 설정된 경우)은 `loading.js` 사용 여부에 따라 다릅니다. 공유 레이아웃에서 첫 번째 `loading.js` 파일까지의 렌더링된 "트리"만 프리페치되고 30초 동안 캐시됩니다. 이는 전체 동적 라우트를 가져오는 비용을 줄이고, 사용자에게 더 나은 시각적 피드백을 위한 즉각적인 로딩 상태를 보여줄 수 있음을 의미합니다.

`prefetch` prop을 `false`로 설정하여 프리페칭을 비활성화할 수 있습니다. 또는 `prefetch` prop을 `true`로 설정하여 로딩 경계를 넘어 전체 페이지 데이터를 프리페치할 수 있습니다.

자세한 내용은 `<Link>` API 참조를 확인하세요.


{% endcode %}

{% code title="알아두면 좋은 점" %}

```
- 프리페칭은 개발 환경에서는 활성화되지 않고 프로덕션 환경에서만 활성화됩니다.

```

{% endcode %}

### Cache

Next.js에는 Router Cache라고 불리는 인메모리 클라이언트 사이드 캐시가 있습니다. 사용자가 앱을 탐색할 때, 프리페치된 라우트 세그먼트와 방문한 라우트의 React Server Component Payload가 이 캐시에 저장됩니다.

이는 내비게이션 시 서버에 새로운 요청을 하는 대신 가능한 한 캐시를 재사용한다는 것을 의미합니다. 이를 통해 요청 횟수와 전송되는 데이터량을 줄여 성능을 향상시킵니다.

Router Cache의 작�� 방식과 구성 방법에 대해 자세히 알아보려면 [여기]("https://nextjs.org/docs/app/building-your-application/caching#router-cache")를 클릭하세요.

### Partial Rendering

부분 렌더링은 내비게이션 시 변경되는 라우트 세그먼트만 클라이언트에서 다시 렌더링되고, 공유된 세그먼트는 유지되는 것을 의미합니다.

예를 들어, `/dashboard/settings`와 `/dashboard/analytics`라는 두 형제 라우트 사이를 이동할 때, settings와 analytics 페이지만 렌더링되고 공유된 dashboard 레이아웃은 유지됩니다.

<figure>
  <img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fpartial-rendering.png&w=3840&q=75&dpl=dpl_4kWRRdpV5mEWMp9Zhahs8vP5fgBq" alt="부분 렌더링 예시">
  <figcaption>부분 렌더링 예시</figcaption>
</figure>

부분 렌더링이 없다면, 각 내비게이션마다 클라이언트에서 전체 페이지를 다시 렌더링해야 할 것입니다. 변경되는 세그먼트만 렌더링함으로써 전송되는 데이터량과 실행 시간을 줄여 성능을 향상시킵니다.

### Soft Navigation

브라우저는 페이지 간 이동 시 "하드 네비게이션"을 수행합니다. Next.js App Router는 페이지 간 "소프트 네비게이션"을 활성화하여 변경된 라우트 세그먼트만 다시 렌더링되도록 합니다(부분 렌더링). 이는 내비게이션 시 클라이언트 React 상태를 유지하는 데 도움이 됩니다.

### Back and Forward Navigation

Next.js는 기본적으로 뒤로/앞으로 내비게이션 시 스크롤 위치를 유지하고 Router Cache의 세그먼트를 재사용합니다.

### Routing between `pages/` and `app/`

`pages/`에서 `app/`로 점진적으로 마이그레이션할 때, Next.js 라우터는 두 방식 사이의 하드 네비게이션을 자동으로 처리합니다. `pages/`에서 `app/`로의 전환을 감지하기 위해 클라이언트 라우터 필터가 있는데, 이는 app 라우트에 대한 확률적 검사를 활용합니다. 이로 인해 간혹 거짓 양성(false positive)이 발생할 수 있습니다. 기본적으로 이러한 경우는 매우 드물어야 하며, 거짓 양성 가능성을 0.01%로 설정합니다. 

이 가능성은 `next.config.js` 파일의 `experimental.clientRouterFilterAllowedRate` 옵션을 통해 사용자 정의할 수 있습니다. 거짓 양성 비율을 낮추면 클라이언트 번들에서 생성된 필터의 크기가 증가한다는 점을 유의해야 합니다.

대안으로, 이 처리를 완전히 비활성화하고 `pages/`와 `app/` 사이의 라우팅을 수동으로 관리하고 싶다면, `next.config.js`에서 `experimental.clientRouterFilter`를 false로 설정할 수 있습니다. 이 기능을 비활성화하면, app 라우트와 겹치는 pages의 동적 라우트는 기본적으로 제대로 탐색되지 않습니다.
