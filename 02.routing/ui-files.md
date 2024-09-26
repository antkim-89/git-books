---
description: UI에 관련된 파일과 사용법
---

# UI 관련 파일들

## Pages

page 파일은 경로(폴더)당 내부에 단일로 존재해야합니다. page파일로 경로의 View를 표현할 수 있습니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fpage-special-file.png&w=1920&q=75" alt=""><figcaption><p>page 파일 예시</p></figcaption></figure>

{% endcode %}

{% code title="typescript" %}

```typescript
// `app/page.tsx` is the UI for the `/` URL
export default function Page() {
  return <h1>Hello, Home page!</h1>;
}
```

{% endcode %}
