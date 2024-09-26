# basic files

### 최상위 레벨 파일

최상위 레벨 파일들은 nextjs를 구성하고, 의존성을 관리하며, 미들웨어를 실행시키고, 환경 변수들을 관리합니다.

| 파일명                                                                                                           | 설명                                                                |
| ------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| [`next.config.js`](https://nextjs.org/docs/app/api-reference/next-config-js)                                  | nextjs를 구성파일                                                      |
| [`package.json`](https://nextjs.org/docs/getting-started/installation#manual-installation)                    | 프로젝트의 의존성과 스크립트 구성파일                                              |
| [`instrumentation.ts`](https://nextjs.org/docs/app/building-your-application/optimizing/instrumentation)      | OpenTelemetry[^1]와 [Instrumentation file](#user-content-fn-2)[^2] |
| [`middleware.ts`](https://nextjs.org/docs/app/building-your-application/routing/middleware)                   | Next.js request 미들웨어                                              |
| [`.env`](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables)             | 환경 변수들                                                            |
| [`.env.local`](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables)       | 로컬 환경 변수들                                                         |
| [`.env.production`](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables)  | 프로덕트 환경 변수들                                                       |
| [`.env.development`](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables) | 개발 환경 변수들                                                         |
| [`.eslintrc.json`](https://nextjs.org/docs/app/building-your-application/configuring/eslint)                  | ESlint 구성 파일                                                      |
| `.gitignore`                                                                                                  | 깃에 공유하지 않을 파일 설정                                                  |
| `next-env.d.ts`                                                                                               | nextjs를 위한 타입스크립트 정의파일                                            |
| `tsconfig.json`                                                                                               | 타입스크립트 설정 파일                                                      |

### 라우팅 파일

|                                                                                                   |                     |                    |
| ------------------------------------------------------------------------------------------------- | ------------------- | ------------------ |
| [`layout`](https://nextjs.org/docs/app/api-reference/file-conventions/layout)                     | `.js` `.jsx` `.tsx` | 레이아웃               |
| [`page`](https://nextjs.org/docs/app/api-reference/file-conventions/page)                         | `.js` `.jsx` `.tsx` | 페이지                |
| [`loading`](https://nextjs.org/docs/app/api-reference/file-conventions/loading)                   | `.js` `.jsx` `.tsx` | 로딩 UI              |
| [`not-found`](https://nextjs.org/docs/app/api-reference/file-conventions/not-found)               | `.js` `.jsx` `.tsx` | 페이지 없음 UI          |
| [`error`](https://nextjs.org/docs/app/api-reference/file-conventions/error)                       | `.js` `.jsx` `.tsx` | 에러 UI              |
| [`global-error`](https://nextjs.org/docs/app/api-reference/file-conventions/error#global-errorjs) | `.js` `.jsx` `.tsx` | 글로벌 에러 UI          |
| [`route`](https://nextjs.org/docs/app/api-reference/file-conventions/route)                       | `.js` `.ts`         | API 엔드포인트          |
| [`template`](https://nextjs.org/docs/app/api-reference/file-conventions/template)                 | `.js` `.jsx` `.tsx` | Re-rendered layout |
| [`default`](https://nextjs.org/docs/app/api-reference/file-conventions/default)                   | `.js` `.jsx` `.tsx` | 병렬 라우팅 대체 페이지      |



[^1]: **Open과 Telemetry 의 조합어로 말 그대로 모든 것이 열려있는 개방적인 모니터링 도구**

[^2]: 모니터링 도구 설정 파일
