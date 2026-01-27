강의 자료 참고
https://gymcoding.notion.site/Next-js-Notion-with-Course-AI-1d06a10d310b80be8097e3547fd8efff

## 20. ESLint, Prettier 설정
global.css 환경 설정  
CSS > Lint: Unknown At Rule -> ignore

package.json script 추가
```
"lint:fix": "next lint --fix"
```

## 21. TailwindCSS v4 최신 설치 및 최적화

```bash
$ npx @tailwindcss/upgrade
```

수동 적용
https://tailwindcss.com/docs/installation/using-postcss

- 확장 플러그인 설치
  - eslint
  - prettier
  - Tailwind CSS IntelliSense

플러그인 설치
```bash
npm install tailwindcss-animate postcss

npm install --save-dev prettier eslint-config-prettier

npm install -D prettier prettier-plugin-tailwindcss
```

`.prettierrc` 파일에 추가
```json
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

## 22. shadcn/ui 설치 (wit Layout UI)
https://gymcoding.notion.site/shadcn-ui-wit-Layout-UI-19a6a10d310b80b4a935da1acc070dd4

```bash
npx shadcn@latest init
```

card 컴포넌트 추가
```bash
npx shadcn@latest add card
```

## 29. 동적 라우트 (Dynamic Route)

badge, button, separator 컴포넌트 추가
```bash
npx shadcn@latest add badge button separator
```