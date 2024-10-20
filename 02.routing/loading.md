---
description: Next.js loading 개념
---

# Loading UI and Streaming

특별한 파일인 `loading.js`는 [React Suspense](https://react.dev/reference/react/Suspense)를 사용하여 의미 있는 로딩 UI를 만드는 데 도움을 줍니다. 

이 규칙을 사용하면 라우트 세그먼트의 내용이 로드되는 동안 서버에서 즉각적인 로딩 상태를 표시할 수 있습니다. 렌더링이 완료되면 새로운 내용이 자동으로 교체됩니다.
