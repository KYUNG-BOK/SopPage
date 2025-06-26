# 📄 React PDF Viewer 페이지 구현

이 프로젝트는 React 기반으로 **PDF 파일을 웹에서 뷰어 형태로 출력**하는 기능을 구현하여 공유합니다.  
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

## 🧩 설치된 주요 라이브러리

```bash
npm install @react-pdf-viewer/core @react-pdf-viewer/default-layout pdfjs-dist@2.16.105 --legacy-peer-deps
```

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
2. 위 코드가 포함된 페이지를 `/sop` 등 라우터에 연결  
3. 아래 명령어로 실행 후 브라우저에서 확인

```bash
npm run dev
```

---

## 🎓 구현 후기

> 처음으로 PDF 파일을 웹에서 렌더링해보는 경험이었고,  
> 라이브러리의 사용법과 PDF.js의 동작 원리를 조금씩 익힐 수 있었습니다.  
> 추후에는 클릭 시 모달로 띄우는 방식 등 다양한 확장도 가능하려고 생각하고 있습니다.
