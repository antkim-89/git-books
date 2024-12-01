---
description: Next.js redirect 개념
---

# Redirecting

Next.js에서 리다이렉트를 처리하는 방법은 몇 가지가 있습니다. 이 페이지는 각 옵션의 사용 사례와 많은 수의 리다이렉트를 관리하는 방법을 다룹니다.

| API                                                                                               | Purpose             | Where                    |    Status Code  |             
| --------------------------------------------------------------------------------- | ------------------- | -----------------------  | --------------- |
| [`redirect`](https://nextjs.org/docs/app/api-reference/file-conventions/layout)  | 	변경이나 이벤트 후 사용자 리다이렉트  | 서버 컴포넌트, 서버 액션, 라우트 핸들러   |        307 (임시) 또는 303 (서버 액션)         |
| [`permanentRedirect`](https://nextjs.org/docs/app/api-reference/file-conventions/layout)  | 	변경이나 이벤트 후 사용자 리다이렉트  | 서버 컴포넌트, 서버 액션, 라우트 핸들러   |        308 (영구)         |
| [`useRouter`](https://nextjs.org/docs/app/api-reference/file-conventions/layout)  | 	클라이언트 사이드 네비게이션 수행  | 클라이언트 컴포넌트의 이벤트 핸들러   |        N/A         |
| [`redirects` in `next.config.js`](https://nextjs.org/docs/app/api-reference/file-conventions/layout)  | 	경로 기반 들어오는 요청 리다이렉트  | next.config.js 파일   |        307 (임시) 또는 308 (영구)         |
| [`NextResponse.redirect`](https://nextjs.org/docs/app/api-reference/file-conventions/layout)  | 	조건에 따른 들어오는 요청 리다이렉트  | 미들웨어   |        모든 상태 코드         |

## `redirect` function


`redirect` 함수는 사용자를 다른 URL로 리다이렉트할 수 있습니다. 서버 컴포넌트, 라우트 핸들러, 서버 액션에서 `redirect` 함수를 호출할 수 있습니다.

`redirect` 함수는 변경이나 이벤트 후 사용자를 리다이렉트할 때 자주 사용됩니다. 예를 들어, 게시물을 생성한 후 사용자를 리다이렉트하는 경우:

{% endcode %}

{% code title="app/actions.tsx" %}

```js
'use server'
 
import { redirect } from 'next/navigation'
import { revalidatePath } from 'next/cache'
 
export async function createPost(id: string) {
  try {
    // database 호출
  } catch (error) {
    // 에러 처리
  }
 
  revalidatePath('/posts') // 캐시된 게시물 업데이트
  redirect(`/post/${id}`) // 새로운 게시물 페이지로 이동
}
```

{% endcode %}

{% endcode %}

{% code title="체크 사항" %}


- redirect는 기본적으로 307 (임시 리다이렉트) 상태 코드를 반환합니다. 서버 액션에서는 303 (다른 페이지 보기)를 반환하며, 이는 일반적으로 POST 요청 결과로 성공 페이지로 리다이렉트하는 데 사용됩니다.

- `redirect`는 내부적으로 오류를 발생시키므로 try/catch 블록 외부에서 호출되어야 합니다.

- `redirec`t는 렌더링 과정에서 클라이언트 컴포넌트에서 호출할 수 있지만, 이벤트 핸들러에서는 호출할 수 없습니다. 대신 `useRouter` 훅을 사용할 수 있습니다.

- `redirect`는 절대 URL을 받아들이며, 외부 링크로 리다이렉트하는 데 사용할 수 있습니다.

- 렌더링 과정 전에 리다이렉트하려면 `next.config.js` 또는 미들웨어를 사용하세요.


{% endcode %}


