---
description: UI에 관련된 파일과 사용법
---

# UI Files

UI를 구성하는 기본 적인 파일들입니다.

앞으로 UI 구성함에 있어 각 파일 간의 차이점을 잘 구분지어 사용하는 방법을 익힐 것 입니다.

## Page

page 파일은 경로(폴더)당 내부에 단일로 존재해야합니다. page파일로 경로의 View를 표현할 수 있습니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fpage-special-file.png&w=1920&q=75" alt=""><figcaption><p>page 파일 예시</p></figcaption></figure>

{% endcode %}

{% code title="typescript" %}

```typescript
// `app/page.tsx`는 루트경로(`/`) UI를 위한 파일입니다.
export default function Page() {
  return <h1>Hello,Root!</h1>;
}
```

{% endcode %}

{% code title="체크 사항" %}


경로(폴더)당 단일로 존재해야 합니다.
page 파일이 없으면 해당 경로로 퍼블릭한 접근을 할 수 없습니다.
기본적으로 서버 컴포넌트로 설정되지만, 클라이언트 컴포넌트로 설정할 수 있습니다.
페이지는 데이터 패칭이 가능합니다.


{% endcode %}

## Layouts

layout 파일로는 `layout`와 `template`이 있습니다. 이 두 파일은 항상 같은 경로 내의 공유된 UI를 생성합니다.

### Layout

`layout`은 여러 경로에서 공유되는 UI입니다. 탐색 중에 상태를 유지하고, 상호작용이 가능하며, 리렌더링하지 않습니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Flayout-special-file.png&w=1920&q=75" alt=""><figcaption><p>layout 위치 예시</p></figcaption></figure>

{% endcode %}

{% code title="layout 예시" %}

```js
export default function DashboardLayout({
  children, // 종속 되어 있는 페이지나 하위 레이아웃
}: {
  children: React.ReactNode,
}) {
  return (
    <section>
      {/* 공유되는 UI나 헤더, 네비게이션등 */}
      <nav></nav>
      {children}
    </section>
  );
}
```

{% endcode %}

`Root Layout`은 최상위 루트인 `app` 디렉토리에 선언되며 모든 페이지를 포함하고, 루트 레이아웃은 html과 body 태그를 반드시 포함하여야 합니다.

{% endcode %}

{% code title="root layout 예시" %}

```js
export default function RootLayout({
  children,
}: {
  children: React.ReactNode,
}) {
  return (
    <html lang="en">
      <body>
        {/* Layout UI */}
        <main>{children}</main>
      </body>
    </html>
  );
}
```

{% endcode %}

### Nesting Layout

레이아웃은 중첩이 가능합니다. 중첩된 레이아웃은 상위 레이아웃에서 하위 레이아웃을 포함하며 아래와 같은 모양 표시됩니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fnested-layouts-ui.png&w=1920&q=75" alt=""><figcaption><p>nesting layout 위치 예시</p></figcaption></figure>

{% endcode %}

{% code title="체크 사항" %}
```
- 루트 레이아웃만이 <html>과 <body>를 포함할 수 있습니다.

- 기본적으로 서버 컴포넌트로 설정되지만, 클라이언트 컴포넌트로 설정할 수 있습니다. 레이아웃은 데이터 패칭이 가능합니다.

- 부모 레이아웃과 자식 간에 데이터를 직접 전달할 수는 없습니다. 하지만 동일한 데이터를 경로에서 여러 번 가져오는 것은 가능하며, React는 성능에 영향을 주지 않고 이러한 요청을 자동으로 중복 제거(dedupe)합니다.

- layout에서는 pathname에 직접 접근이 불가능합니다. 하지만 주입된 클라이언트 컴포넌트의 경우 usePathname hook을 사용할 수 있습니다.

- 레이아웃은 자신 아래의 경로 세그먼트에 접근할 수 없습니다. 모든 경로 세그먼트에 접근하려면 클라이언트 컴포넌트에서 useSelectedLayoutSegment 또는 useSelectedLayoutSegments를 사용할 수 있습니다.

- Route Groups를 사용하면 특정 경로 세그먼트를 공유 레이아웃에 포함하거나 제외할 수 있습니다.

- Route Groups를 사용하면 여러 개의 루트 레이아웃을 생성할 수 있습니다. 이를 통해 경로 세그먼트를 그룹화하고 각 그룹에 대해 별도의 레이아웃을 설정할 수 있습니다. 이는 보다 복잡한 레이아웃 구조를 유연하게 구성하는 데 유용합니다.

- 페이지 디렉토리에서 마이그레이션할 때, 루트 레이아웃은 기존의 _app.js와 _document.js 파일을 대체합니다. 마이그레이션 가이드를 참조하여 전환 과정을 확인할 수 있습니다.
```

{% endcode %}

### Template

`template`는 `layout`과 유사하게 작동하지만, 탐색 시에 매번 새로운 인스턴스를 만들어냅니다. 그렇기 때문에 컴포넌트의 상태가 유지되지 않으며, `useEffect`는 다시 동기화됩니다. 

이렇게 재동기화가 필요한 경우엔 `layout`보단 `template`을 활용하는 것이 유리합니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Ftemplate-special-file.png&w=1920&q=75" alt=""><figcaption><p>template 위치 예시</p></figcaption></figure>

template는 `layout`과 자식 컴포넌트 사이에 위치하여 렌더링됩니다. 예를 들어, 탐색 시 `layout`이 상태를 유지하는 동안, `template`은 자식 컴포넌트의 새 인스턴스를 생성합니다. 

이를 통해 `template`이 `layout`과 자식 사이의 중개 역할을 하며, 자식 컴포넌트는 초기화됩니다.

{% endcode %}

{% code title="Output 예시" %}

```js
<Layout>
  {/* Note that the template is given a unique key. */}
  <Template key={routeParam}>{children}</Template>
</Layout>
```

{% endcode %}

## ETC(meta, nav link)

### Metadata

Next.js에서는 Metadata api를 통해서 `<head>`, `title`와 `meta`등의 HTML 엘리먼트를 설정할 수 있습니다. 아래의 예시를 보면 이해하기 편합니다.

{% endcode %}

{% code title="app/page.tsx" %}

```js
import type { Metadata } from "next";

export const metadata: Metadata = {
  title: "Next.js",
};

export default function Page() {
  return "...";
}
```

{% endcode %}

### Active Nav Links

`usePathname()`이란 훅(hook)을 사용하여 네비게이션의 활성을 정의할 수 있습니다.

`usePathname()`은 클라이언트 훅이기 때문에 `use client`정의를 잊으면 안됩니다.

{% endcode %}

{% code title="Nav 예시" %}

```js
"use client";

import { usePathname } from "next/navigation";
import Link from "next/link";

export function NavLinks() {
  const pathname = usePathname();

  return (
    <nav>
      <Link className={`link ${pathname === "/" ? "active" : ""}`} href="/">
        Home
      </Link>

      <Link
        className={`link ${pathname === "/about" ? "active" : ""}`}
        href="/about">
        About
      </Link>
    </nav>
  );
}
```

{% endcode %}
