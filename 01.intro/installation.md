# installation

System Requirements:

* [Node.js 18.18](https://nodejs.org/)나 그 이후 버전.
* macOS, Windows (including WSL),와 Linux가 지원됌



### 설치방법

next는 `create-next-app` 명령어로 시작합니다.

{% code title="terminal" %}
```sh
npx create-next-app@latest
```
{% endcode %}

{% code title="terminal" %}
```powershell
What is your project named? my-app 
- 이름 설정

Would you like to use TypeScript? No / Yes 
- 타입스크립트 적용 여부(적용 추천)

Would you like to use ESLint? No / Yes 
- lint 적용 여부(적용 추천)

Would you like to use Tailwind CSS? No / Yes 
- tailwind 적용 여부(적용 추천)

Would you like your code inside a `src/` directory? No / Yes 
- source 디렉토리 생성여부(취향)

Would you like to use App Router? (recommended) No / Yes 
- 14 이후 버전부터는 app라우팅이 추천됨

Would you like to use Turbopack for `next dev`?  No / Yes 
- Turbopack 패키징 (취향)

Would you like to customize the import alias (`@/*` by default)? No / Yes 
- 커스텀 경로 (적용 추천)

What import alias would you like configured? @/* 
- 커스텀 경로 설정 (적용 추천)
```
{% endcode %}

### 디렉토리 구조

명령어로 nextjs설치하고 나면 아래와 같은 구조로 디렉토리가 생깁니다.

그리고 몇개의 파일들이 생성되는데 가장 기본이 되는 파일들입니다.

{% code title="directory" %}
```
-src
    -app
        -layout.tsx -- 루트 페이지 레이아웃
        -app.tsx -- 루트 페이지
        ...

...
-package.json -- 현재 프로젝트에 대한 정보, 설치된 패키지와 설정된 스크립트 정보 등의 파일.
-tsconfig.json -- 타입스크립트 설정 파일.
-tailwind.config.ts -- 테일윈드 설정 파일.

```
{% endcode %}

