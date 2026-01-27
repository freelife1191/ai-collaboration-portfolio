# Zenthor 랜딩 페이지 제작

- AI툴: Google AI Studio & Antigravity
- 배포: Netlify
- 소요시간: 30분
- GitHub: https://github.com/freelife1191/zenthor-randing-page
- Demo: https://zenthor.netlify.app/

```
<iframe src='https://my.spline.design/robotarm-wp9meEDdh4CQSR60cLNe8F8H/' frameborder='0' width='100%' height='100%'></iframe>
내가 주는 spline 3d와 이미지를 가지고 멋진 피지컬 AI 산업 자동화 로봇 웹사이트를 만들어보자. 디자인을 예쁘게 만들어서 고급스러운 느낌이 들게 해줘. 브랜드 명은 Zenthor 로 만들어줘.
```

```
spline 3d 변경해줘 그리고 영역이 spline 3d 영역이 너무 넓어서 아래로 페이지 드래그가 잘안돼고 있어. spline 3d 영역 조정좀 해줘
<script type="module" src="https://unpkg.com/@splinetool/viewer@1.12.36/build/spline-viewer.js"></script>
<spline-viewer url="https://prod.spline.design/OZLlsHkQZsKQqIYv/scene.splinecode"></spline-viewer>
```

```
spline 3d Orbit 이 동작하지 않아 pointer-events-none 속성을 제거 하고 spline 3d 상하 폭을 좀 줄여줘 검은 여백이 너무 많은거 같아
<script type="module" src="https://unpkg.com/@splinetool/viewer@1.12.36/build/spline-viewer.js"></script>
<spline-viewer url="https://prod.spline.design/OZLlsHkQZsKQqIYv/scene.splinecode"></spline-viewer>
```

```
Precision , Intuition. 문구가 잘 안보여 글씨 색상을 밝게 해서 잘 보이게 해주고
하단에 사용하는 회사들 도 잘 안보여 하단 사용하는 회사들 글씨 색상은 어둡게 해서 잘보이게 해줘
```

소스 GitHub 배포 후 다운 받아 Antigravity 로 배포
```
Netlify 배포 환경 구성해서 배포 진행해줘
```