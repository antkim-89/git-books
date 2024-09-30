---
description: UI에 관련된 파일과 사용법
---

# UI 관련 파일들

### Pages

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

```
경로(폴더)당 단일로 존재해야 합니다.
page 파일이 없으면 해당 경로로 퍼블릭한 접근을 할 수 없습니다.
기본적으로 서버 컴포넌트로 설정되지만, 클라이언트 컴포넌트로 설정할 수 있습니다.
페이지는 데이터 패칭이 가능합니다.
```

{% endcode %}

### Layouts

layout 파일로는 `layout`와 `template`이 있습니다. 이 두 파일은 항상 같은 경로 내의 공유된 UI를 생성합니다.

#### Layout

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
