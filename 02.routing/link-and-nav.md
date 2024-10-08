---
description: Nextjs에서 네비게이팅 하는 법
---

# Link and Navigating

Nextjs 내에서 라우팅을 할 수 있는 방법은 4가지입니다.

• `<Link>` 컴포넌트를 이용하는 방법

• `useRouter` 훅을 사용하는 방법 (클라이언트)

• `redirect` 기능을 이용하는 방법 (서버)

• `History API`를 이용하는 방법

이 방법들을 어떻게 사용하는지 익혀봅시다.

## `<Link>` Compoent

`<Link>`의 경우 `prefetching`기능과 라우트 간의 클라이언트 사이드 네비게이팅을 위해 `<a>` 태그로 만들어진 컴포넌트입니다.

Nextjs에서 가장 추천하는 라우팅 방식입니다.

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

- redirect internally throws an error so it should be called outside of try/catch blocks.

- redirect can be called in Client Components during the rendering process but not in event handlers. You can use the useRouter hook instead.

- redirect also accepts absolute URLs and can be used to redirect to external links.

- If you'd like to redirect before the render process, use next.config.js or Middleware.
```

{% endcode %}
