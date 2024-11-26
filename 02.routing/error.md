---
description: Next.js에서 에러 처리하는 법
---

# Error Handling

오류는 예상되는 오류와 포착되지 않은 예외라는 두 가지 범주로 나눌 수 있습니다.

- 리턴 값에 에러가 예상되는 경우 : Server Action에서 예상되는 오류에 `try`/`catch`를 사용하지 않습니다. `useFormState`를 사용하여 이러한 오류를 관리하고 클라이언트에 반환합니다.

- 예기치 못하는 에러의 경우 : `error.tsx` 및 `global-error.tsx` 파일을 사용하여 예기치 않은 오류를 처리하고 대체 UI를 제공합니다.

## Handling Expected Errors

예상되는 오류는 애플리케이션의 정상 작동 중에 발생할 수 있는 오류입니다. 예를 들어, 서버 측 폼 유효성 검사 또는 실패한 요청에서 발생할 수 있습니다. 이러한 오류는 명시적으로 처리되고 클라이언트에 반환되어야 합니다.

### Handling Expected Errors from Server Actions

`useFormState` 훅을 사용하여 Server Action의 상태를 관리하고 오류를 처리합니다. 이 접근 방식은 예상되는 오류에 대한 `try`/`catch` 블록을 피하고 예외 대신 반환 값으로 모델링합니다.

{% endcode %}

{% code title="app/actions.ts" %}

```js
'use server'
 
import { redirect } from 'next/navigation'
 
export async function createUser(prevState: any, formData: FormData) {
  const res = await fetch('https://...')
  const json = await res.json()
 
  if (!res.ok) {
    return { message: 'Please enter a valid email' }
  }
 
  redirect('/dashboard')
}
```

{% endcode %}

그런 다음, 오류 메시지를 표시하기 위해 `useFormState` 훅에 작업을 전달하고 반환된 상태를 사용할 수 있습니다.

{% endcode %}

{% code title="app/ui/signup.tsx" %}

```js
'use client'
 
import { useFormState } from 'react-dom'
import { createUser } from '@/app/actions'
 
const initialState = {
  message: '',
}
 
export function Signup() {
  const [state, formAction] = useFormState(createUser, initialState)
 
  return (
    <form action={formAction}>
      <label htmlFor="email">Email</label>
      <input type="text" id="email" name="email" required />
      {/* ... */}
      <p aria-live="polite">{state?.message}</p>
      <button>Sign up</button>
    </form>
  )
}
```

{% endcode %}

{% endcode %}

{% code title="체크 사항" %}

```markdown
이 예제는 React의 useFormState 훅을 사용하며, Next.js App Router와 함께 제공됩니다.

React 19를 사용하는 경우 useActionState를 대신 사용하세요.

자세한 내용은 React 문서를 참조하세요.
```


{% endcode %}

반환된 상태를 사용하여 클라이언트 컴포넌트에서 토스트 메시지를 표시할 수 있습니다.

### Handling Expected Errors from Server Components

서버 컴포넌트 내에서 데이터를 가져올 때, 응답을 사용하여 조건부로 오류 메시지 또는 [`redirect`](../02.routing/basic.md)를 렌더링할 수 있습니다.


{% endcode %}

{% code title="app/page.tsx" %}

```js
export default async function Page() {
  const res = await fetch(`https://...`)
  const data = await res.json()
 
  if (!res.ok) {
    return 'There was an error.'
  }
 
  return '...'
}
```

{% endcode %}

## Uncaught Exceptions

포착되지 않은 예외는 애플리케이션의 정상적인 흐름 중에 발생해서는 안 되는 버그나 문제를 나타내는 예기치 않은 오류입니다. 이는 오류를 발생시켜 처리해야 하며 오류 경계에 의해 포착됩니다.

- 공통: `error.js`를 사용하여 루트 레이아웃 아래에서 발견되지 않은 오류를 처리합니다.

- 선택사항: 중첩된 `error.js` 파일(예: `app/dashboard/error.js`)을 사용하여 포착되지 않은 세부적인 오류를 처리합니다.

- 일반적이지 않음: `global-error.js`를 사용하여 루트 레이아웃에서 발견되지 않은 오류를 처리합니다.

### Using Error Boundaries

Next.js는 error boundaries를 사용하여 포착되지 않은 예외를 처리합니다. Error boundaries는 하위 구성요소의 오류를 포착하고 충돌이 발생한 구성요소 트리 대신 대체 UI를 표시합니다.

라우트 세그먼트 내에 `error.tsx` 파일을 추가하고 React 구성요소를 내보내어 error boundary를 만듭니다.

{% endcode %}

{% code title="app/page.tsx" %}

```js
export default async function Page() {
  const res = await fetch(`https://...`)
  const data = await res.json()
 
  if (!res.ok) {
    return 'There was an error.'
  }
 
  return '...'
}
```

{% endcode %}