---
description: Next.js loading 개념
---

# Loading UI and Streaming

특별한 파일인 `loading.js`는 [React Suspense](https://react.dev/reference/react/Suspense)를 사용하여 의미 있는 로딩 UI를 만드는 데 도움을 줍니다.

이 규칙을 사용하면 라우트 세그먼트의 내용이 로드되는 동안 서버에서 즉각적인 로딩 상태를 표시할 수 있습니다. 렌더링이 완료되면 새로운 내용이 자동으로 교체됩니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Floading-ui.png&w=1920&q=75&dpl=dpl_4kWRRdpV5mEWMp9Zhahs8vP5fgBq" alt=""><figcaption></figcaption></figure>

## Instant Loading States

즉각적인 로딩 상태는 탐색 시 즉시 표시되는 폴백 UI입니다. 스켈레톤과 스피너와 같은 로딩 인디케이터를 미리 렌더링하거나, 커버 사진, 제목 등 미래 화면의 작은 부분을 표시할 수 있습니다.

이는 사용자가 앱이 응답하고 있음을 이해하고 더 나은 사용자 경험을 제공합니다.

폴더 내부에 `loading.js` 파일을 추가하여 로딩 상태를 만듭니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Floading-special-file.png&w=1920&q=75&dpl=dpl_4kWRRdpV5mEWMp9Zhahs8vP5fgBq" alt=""><figcaption></figcaption></figure>

{% endcode %}

{% code title="app/dashboard/loading.tsx" %}

```js
export default function Loading() {
  // You can add any UI inside Loading, including a Skeleton.
  return <LoadingSkeleton />
}
```

{% endcode %}

같은 폴더에 `loading.js` 파일이 있으면, `layout.js` 내부에 중첩됩니다. 이는 자동으로 `page.js` 파일과 그 아래의 모든 자식을 `<Suspense>` 경계로 감쌉니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Floading-overview.png&w=1920&q=75&dpl=dpl_4kWRRdpV5mEWMp9Zhahs8vP5fgBq" alt=""><figcaption></figcaption></figure>

{% endcode %}

{% code title="체크 사항" %}

```
- 서버 중심 라우팅을 사용하더라도 내비게이션이 즉각적으로 이루어집니다.

- 내비게이션은 중단 가능합니다. 즉, 경로를 변경할 때 현재 경로의 콘텐츠가 완전히 로드될 때까지 기다릴 필요 없이 다른 경로로 이동할 수 있습니다.

- 새로운 경로 세그먼트가 로드되는 동안에도 공유 레이아웃은 계속 상호작용이 가능한 상태를 유지합니다.
```

{% endcode %}

```
추천: 라우트 세그먼트(레이아웃 및 페이지)에 `loading.js` 규칙을 사용하세요. Next.js는 이 기능을 최적화하기 때문입니다.
```

## Streaming with Suspense

`loading.js` 외에도, 직접 자신의 UI 컴포넌트에 대한 [Suspense](https://react.dev/reference/react/Suspense) 경계를 만들 수 있습니다. App Router는 [Node.js 및 Edge 런타임](https://nextjs.org/docs/app/building-your-application/rendering/edge-and-nodejs-runtimes)에서 Suspense를 사용한 스트리밍을 지원합니다.

{% endcode %}

{% code title="체크 사항" %}

```
- 일부 브라우저는 스트리밍 응답을 버퍼링합니다. 스트리밍 응답을 볼 수 있을 때까지 1024바이트를 초과할 때까지 볼 수 없습니다. 이는 일반적으로 “hello world” 애플리케이션에만 영향을 미치지만 실제 애플리케이션에는 영향을 미치지 않습니다.
```

{% endcode %}

## What is Streaming?

스트리밍은 데이터를 즉시 전송하는 대신 일부 데이터를 버퍼링하는 것을 의미합니다. 이는 데이터가 준비되는 즉시 전송되는 것을 의미합니다. 이는 데이터가 준비되는 즉시 전송되는 것을 의미합니다.

스트리밍이 작동하는 방식을 이해하려면, Server-Side Rendering (SSR) 및 그 한계를 이해하는 것이 도움이 됩니다.

SSR을 사용할 때, 사용자가 페이지를 보고 상호작용할 수 있기 전에 완료해야 할 일련의 단계가 있습니다:

1. 먼저 서버에서 해당 페이지에 필요한 모든 데이터를 가져옵니다.

2. 서버에서 페이지의 HTML을 렌더링합니다.

3. 페이지의 HTML, CSS, 그리고 JavaScript를 클라이언트로 전송합니다.

4. 생성된 HTML과 CSS를 사용하여 비대화형 사용자 인터페이스를 표시합니다.

5. 마지막으로, [React hydrates](https://react.dev/reference/react-dom/client/hydrateRoot#hydrating-server-rendered-html)을 수행하여 사용자 인터페이스를 상호작용 가능하게 만듭니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-without-streaming-chart.png&w=3840&q=75&dpl=dpl_4kWRRdpV5mEWMp9Zhahs8vP5fgBq" alt=""><figcaption></figcaption></figure>

이러한 단계는 순차적이고 동기화되어 있습니다. 즉, 서버는 모든 데이터가 가져와지기 전까지 페이지의 HTML을 렌더링할 수 없습니다. 또한, 클라이언트에서 React는 페이지의 모든 컴포넌트에 대한 코드가 다운로드된 후에만 UI를 하이드레이션할 수 있습니다.

SSR과 React 및 Next.js는 사용자에게 가능한 한 빨리 non-interactive 페이지를 표시하여 인지된 로딩 성능을 향상시킵니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-without-streaming.png&w=3840&q=75&dpl=dpl_4kWRRdpV5mEWMp9Zhahs8vP5fgBq" alt=""><figcaption></figcaption></figure>

그러나, 서버에서 모든 데이터 가져오기가 완료되기 전까지 페이지를 사용자에게 표시할 수 없기 때문에 여전히 느릴 수 있습니다.

스트리밍을 사용하면 페이지의 HTML을 더 작은 조각으로 나누고 서버에서 클라이언트로 이러한 조각을 점진적으로 전송할 수 있습니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-with-streaming.png&w=3840&q=75&dpl=dpl_4kWRRdpV5mEWMp9Zhahs8vP5fgBq" alt=""><figcaption></figcaption></figure>

이는 페이지의 일부를 더 빨리 표시할 수 있도록 하여, 모든 데이터가 로드되기 전에 어떤 UI도 렌더링할 수 없는 문제를 해결합니다.

스트리밍은 React의 컴포넌트 모델과 잘 작동하는데, 각 컴포넌트를 조각으로 간주할 수 있기 때문입니다. 높은 우선순위의 컴포넌트(예: 제품 정보) 또는 데이터에 의존하지 않는 컴포넌트(예: 레이아웃)를 먼저 전송할 수 있으며, React는 더 빨리 하이드레이션을 시작할 수 있습니다. 낮은 우선순위의 컴포넌트(예: 리뷰, 관련 제품)는 데이터가 가져와지고 나서 동일한 서버 요청에 함께 전송될 수 있습니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-with-streaming-chart.png&w=3840&q=75&dpl=dpl_4kWRRdpV5mEWMp9Zhahs8vP5fgBq" alt=""><figcaption></figcaption></figure>

스트리밍은 페이지를 렌더링하는 데 긴 데이터 요청이 차단되는 것을 방지하는 데 특히 유용합니다. 이는 [Time To First Byte (TTFB)](https://web.dev/articles/ttfb?hl=ko) 및 [First Contentful Paint (FCP)](https://developer.chrome.com/docs/lighthouse/performance/first-contentful-paint?hl=ko)를 줄이는 데 도움이 되며, 또한 느린 장치에서 [Time to Interactive (TTI)](https://developer.chrome.com/docs/lighthouse/performance/interactive?hl=ko)을 향상시키는 데 도움이 됩니다.

## Example

`<Suspense>`는 비동기 작업(예: 데이터 가져오기)을 수행하는 컴포넌트를 래핑하고, 작업이 진행되는 동안 폴백 UI(예: 스켈레톤, 스피너)를 표시하고, 작업이 완료되면 컴포넌트를 교체합니다.

{% endcode %}

{% code title="app/dashboard/page.tsx" %}

```js
import { Suspense } from 'react'
import { PostFeed, Weather } from './Components'
 
export default function Posts() {
  return (
    <section>
      <Suspense fallback={<p>Loading feed...</p>}>
        <PostFeed />
      </Suspense>
      <Suspense fallback={<p>Loading weather...</p>}>
        <Weather />
      </Suspense>
    </section>
  )
}
```
Suspense를 사용하면 다음과 같은 이점을 얻을 수 있습니다:

1. ***Streaming Server Rendering*** - 서버에서 클라이언트로 HTML을 점진적으로 렌더링합니다.

2. ***Selective Hydration*** - React는 사용자 상호작용을 기반으로 먼저 상호작용 가능한 컴포넌트를 선택합니다.

Suspense에 대한 더 많은 예제와 사용 사례는 [React 문서](https://react.dev/reference/react/Suspense)를 참조하세요.

## SEO

- Next.js는 [`generateMetadata`](https://nextjs.org/docs/app/api-reference/functions/generate-metadata) 내부의 데이터 가져오기가 완료되기 전까지 클라이언트로 UI를 스트리밍합니다. 이는 스트리밍된 응답의 첫 부분이 `<head>` 태그를 포함하는 것을 보장합니다.

- 스트리밍은 서버 렌더링이기 때문에 SEO에 영향을 미치지 않습니다. Google의 [Rich Results Test](https://search.google.com/test/rich-results) 도구를 사용하여 페이지가 Google의 웹 크롤러에 어떻게 표시되는지 확인하고 직렬화된 [HTML (소스)](https://search.google.com/test/rich-results?output=xml&url=https%3A%2F%2Fnextjs.org%2F)을 확인할 수 있습니다.

## Status Codes

스트리밍 중에는 요청이 성공했음을 나타내는 200 상태 코드가 반환됩니다.

서버는 여전히 스트리밍된 콘텐츠 내에서 오류 또는 문제를 클라이언트에 전달할 수 있습니다. 예를 들어, `redirect` 또는 `notFound`를 사용할 때와 같이 응답 헤더가 이미 클라이언트에 전송되었기 때문에 응답의 상태 코드를 업데이트할 수 없습니다. 이는 SEO에 영향을 미치지 않습니다.