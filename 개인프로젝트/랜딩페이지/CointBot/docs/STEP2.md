# 2일차 Cursor로 수정후 Vercel 명령 배포

https://demodev.notion.site/5-with-Cursor-1caccaf841aa81ed94afcb04afdcc08e

- 이미지

## 프론트수정

프롬프트

```
웹 사이트에 헤더를 추가해줘
처음 웹사이트에 접속했을 때만 투명해지고 스크롤하면 헤더에 색이 생겼으면 좋겠어
```

HTML Elements 요소 지정 수정

```
아래 요소의 왼쪽 입력 블록의 크기와 오른쪽 세 개의 블록 크기랑 수평 정렬되게 높이를 키워줘
<p class="text-gray-300 mb-4">"API 연동이 정말 쉬워서 개발자로서도 만족스럽습니다. 퀀트 전략 실험하기 최고예요!"</p>
```

백그라운드 추가

https://kr.pinterest.com/

```
@/public

public 폴더에 있는 'hero-bg.jpg'를 아래 요소의 백그라운드로 써줘

<div class="w-full h-full" style="background-image:linear-gradient(rgba(99, 102, 241, 0.1) 1px, transparent 1px),
                linear-gradient(90deg, rgba(99, 102, 241, 0.1) 1px, transparent 1px);background-size:50px 50px"></div>
```

무료 한글 폰트

https://noonnu.cc/font_page/pick

마음에 드는 폰트 `웹폰트로 사용` 복사

```
현재 웹사이트에 아래 폰트 추가해줘

@font-face {
    font-family: 'RiaSans-ExtraBold';
    src: url('https://fastly.jsdelivr.net/gh/projectnoonnu/2410-1@1.0/RiaSans-ExtraBold.woff2') format('woff2');
    font-weight: normal;
    font-style: normal;
}
```

### 메타 데이터를 넣어서 SEO 최적화 하기

크롬 확장 프로그램 Open Graph 설치 및 사용

https://chromewebstore.google.com/detail/open-graph/nlghkhckpgmmjffajkhocinpnbkhdkoh?hl=ko&utm_source=ext_sidebar

localhost 로 접근해야 동작

http://localhost:3000/


```
@https://nextjs.org/docs/app/building-your-application/optimizing/metadata

이 문서를 참고하여 메타 데이터의 title, descriptrion, Open Graph를 모두 채워줘
Open Graph는 @public 폴더의 'hero-bg.jpg'을 사용해줘
```

에러 해결 - 복사후 붙여넣기

## 7. 배포 및 추적 with Vercel

https://demodev.notion.site/7-with-Vercel-1caccaf841aa812582d6c84e98cae9f7

```
@https://vercel.com/docs/analytics/package

이 링크를 참고하여 현재 프로젝트에 Vercel Analytics를 연결해줘
```

vercel 설치

```bash
# 처음에만 필요
pnpm install -g vercel
vercel login
```

vercel 배포

```bash
vercel --prod

Vercel CLI 46.0.2
? Set up and deploy “~/vive/demodev/coinbot-landing”? yes
? Which scope should contain your project? freeopen1191-4558's projects
? Link to existing project? no
? What’s your project’s name? coinbot-landing
? In which directory is your code located? ./
```

Speed Insights 설치

```bash
pnpm i @vercel/speed-insights
```

Cursor 에 추가 해달라고 함

```js
import { SpeedInsights } from "@vercel/speed-insights/next"
```
