# 📄 React PDF Viewer 페이지 구현

이 프로젝트는 React 기반으로 **PDF 파일을 웹에서 뷰어 형태로 출력**하는 기능을 구현한 예시입니다.  
라이브러리는 `@react-pdf-viewer/core` 와 `pdfjs-dist` 를 사용하였습니다.

---

## 🧐 왜 이걸 만들었나요?

처음에는 단순히 PDF 파일을 웹에서 보여주고 싶다는 필요에서 시작했습니다.  
제가 생각하고 있는 협업 시스템 내에서 **업무 매뉴얼(SOP)** 또는 **가이드 문서(PDF)** 를  
브라우저 안에서 바로 볼 수 있도록 구현하고 싶었기 때문입니다.

PDF를 클릭해서 다운로드하는 방식보다,  
웹 뷰어 형태로 제공하면 사용자 편의성이 더 좋다고 생각했고,  
이를 통해 React에서 외부 라이브러리를 연동해보는 학습 경험도 할 수 있었습니다.

---

## ✅ 주요 기능

- PDF 뷰어 기능 (페이지 넘김, 줌, 썸네일 등 포함)
- PDF 파일은 `/public/assets` 디렉토리에 배치
- 반응형 100% 높이 적용

---

## 🔧 사용된 기술 스택

| 기술 | 설명 |
|------|------|
| **React** + **TypeScript** | 컴포넌트 기반 UI 구성 및 타입 안정성을 제공 |
| **@react-pdf-viewer/core** | 리액트에서 PDF 파일을 렌더링하기 위한 핵심 PDF 뷰어 라이브러리 |
| **@react-pdf-viewer/default-layout** | 툴바, 썸네일, 페이지 넘김 등 포함된 기본 PDF 뷰어 레이아웃 |
| **pdfjs-dist** | Mozilla의 PDF.js를 기반으로 한 PDF 렌더링 백엔드 라이브러리 |
| **Web Worker** | PDF 렌더링을 메인 스레드와 분리하여 성능 향상 |
| **정적 리소스 처리** | `/public/assets/` 폴더에 PDF 파일을 넣고 브라우저에서 직접 렌더링 |
| **100% 반응형 레이아웃** | `style={{ height: '100vh', width: '100%' }}` 를 통해 전체 화면 표시 |

해당 프로젝트에서는 PDF.js 최신 버전과의 호환성 문제로 인해 `pdfjs-dist@2.16.105`를 사용하였으며,  
의존성 충돌을 해결하기 위해 `--legacy-peer-deps` 옵션을 함께 사용하였습니다.

---

## 🧩 설치된 주요 라이브러리

```bash
npm install @react-pdf-viewer/core @react-pdf-viewer/default-layout pdfjs-dist@2.16.105 --legacy-peer-deps
```

---

### ❗ 설치 시 주의사항

--legacy-peer-deps 옵션을 사용한 이유:<br>
react@19 버전과 ag-grid-react, @react-pdf-viewer/* 사이에 peer dependency 충돌이 발생하여 설치 오류가 발생하였습니다.<br>
해당 옵션은 의존성 충돌을 무시하고 강제로 설치하게 도와줍니다.<p>

pdfjs-dist@2.16.105 버전을 고정한 이유:<br>
최신 버전(3.x 이상)은 @react-pdf-viewer/core와 호환되지 않아 아래와 같은 오류가 발생하였습니다.<br>
"The API version 'x.x.x' does not match the Worker version 'y.y.y'"<br>
"Invalid PDF structure"<p>

따라서 2.16.105 버전이 안정적으로 호환되며 정상적으로 실행이 됩니다.<p>

---

## 📁 파일 구조 예시

```
📦 SOPPAGE
├── 📂 public
│   └── 📂 assets
│       └── Semicon.pdf
├── 📂 src
│   └── 📂 pages
│       └── SopPage.tsx
└── 📄 package.json
```

---

## 🧾 핵심 코드 (`SopPage.tsx`)

```tsx
import React from 'react';
import { Worker, Viewer } from '@react-pdf-viewer/core';
import { defaultLayoutPlugin } from '@react-pdf-viewer/default-layout';

import '@react-pdf-viewer/core/lib/styles/index.css';
import '@react-pdf-viewer/default-layout/lib/styles/index.css';

const SopPage: React.FC = () => {
  const defaultLayoutPluginInstance = defaultLayoutPlugin();

  return (
    <div style={{ height: '100vh', width: '100%' }}>
      <Worker workerUrl={`https://unpkg.com/pdfjs-dist@2.16.105/build/pdf.worker.min.js`}>
        <Viewer
          fileUrl="/assets/Semicon.pdf"
          plugins={[defaultLayoutPluginInstance]}
        />
      </Worker>
    </div>
  );
};

export default SopPage;
```

---

## 📝 사용 방법

1. `Semicon.pdf` 파일을 `public/assets` 폴더에 넣기  
2. 위 코드가 포함된 페이지를 `/sop` 혹은 pdf뷰어가 필요한 페이지의 라우터에 연결  
3. 아래 명령어로 실행 후 브라우저에서 확인

```bash
npm run dev
```

---

##  📸 예시이미지 

![bloging](https://github.com/user-attachments/assets/31bdecc2-86f9-4087-add2-02182255bbe7)


---

## 🎓 구현 후기

> 이번 프로젝트는 PDF.js 라이브러리를 사용해 웹에서 PDF 문서를 직접 렌더링해보는 첫 경험이었습니다.  
> 라이브러리의 사용법을 익히는 것뿐만 아니라, PDF.js의 렌더링 메커니즘과 구조를 이해하게 되어 매우 유익했습니다.  
> 특히, HTML `<canvas>` 요소와 PDF 페이지의 연동 과정에서 다양한 시행착오를 겪으면서 웹 프론트엔드와 브라우저 렌더링에 대한 이해도를 높일 수 있었습니다.
> 향후에는 클릭 시 모달 형태로 PDF를 보여주거나, 다중 페이지 지원, 줌 기능 등의 추가 확장도 고려하고 있습니다.
