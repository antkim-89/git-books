---
description: nextjs 라우팅 기본 개념
---

# basic

### 기본 구조

next.js의 기본 라우팅 개념을 이해하는덴 다음 그림을 보시는게 도움이 됩니다.

<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fterminology-component-tree.png&#x26;w=1920&#x26;q=75" alt=""><figcaption><p>NextJs 라우팅 구조도</p></figcaption></figure>

• Tree: 계층 구조를 시각화하는 방식, 예를 들어 부모와 자식 컴포넌트로 이루어진 컴포넌트 트리나 폴더 구조를 의미합니다.

• Subtree: 트리의 일부로, 새로운 루트에서 시작하여 끝 노드(잎)까지 이어지는 구조입니다.

• Root: 트리나 서브트리의 첫 번째 노드로, 예를 들어 루트 레이아웃이 해당됩니다.

• Leaf: 자식이 없는 노드로, 예를 들어 URL 경로의 마지막 세그먼트입니다.



### URL 구조

next js의 URL이 어떻게 구성 되는지 설명하는 그림입니다.



<figure><img src="https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fterminology-url-anatomy.png&#x26;w=1920&#x26;q=75" alt=""><figcaption></figcaption></figure>

* **URL Segment:** URL 경로에서 "/"로 구분되어진 각 요소를 의미합니다. (예: dashboard, setting)
* **URL Path:** 도메인 이후에 씌여진 경로의 일부 입니다. (예:  "/dashboard/setting")
