



## 웹 페이지와 Notion API 연결

```
@Notion 노션 API를 사용하여 웹 사이트의 폼 내용을 노션 데이터베이스에 저장하는 코드를 짜줘
`.env` 파일은 프로젝트의 루트에 만들었고 NOTION_API_KEY, NOTION_DATABASE_ID 두 환경 변수 또한 잘 적어놓았어
```


## 8. Gmail API 키 발급

### Gmail API 설정

Google Cloud Console - API 및 서비스 - 라이브러리 - Gmail API - OAuth 동의 화면

### 승인된 도메인 추가

브랜딩 - make.com 과 integromat.com 을 입력 후 저장

대상 - 테스트 사용자 추가

### 클라이언트 만들기

클라이언트 - 클라이언트 만들기

승인된 리디렉션 URI 항목 추가 - 만들기

- https://www.integromat.com/oauth/cb/google-restricted
- https://us2.make.com/oauth/cb/google-restricted

- Client ID: 1000000000-5XZXXXXXXXXXXX.apps.googleusercontent.com
- Client Security Password: GXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX

### 데이터 엑세스

데이터 엑세스 - 범위 추가 또는 삭제 - API: Gmail API 필터 - 모든 행 선택


## 9. 이메일 자동화 with Make

Crate Senario - Build from scratch

### notion 추가

notion - Watch Database Items - Notion Internal Connect - 시크릿 연동 - Limit 100 - From now on

아래 정보 필요시 입력

NOTION_API_KEY=ntn_4XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
NOTION_DATABASE_ID=2XXXXXXXXXXXXXXXXXXXXXXXXXX

### Gmail 연동

Gmail - Send an Email - Show advanced settings - Show advanced settings

Client ID, Client Secret 입력 - Sign in with Google

From에 **Google Cloud Console에서 Gmail API를 발급한 구글 이메일** 입력 → To 항목의 `Map` 토글 체크 → `Properties Value` 에 `이메일` 선택

Subject 에 안녕하세요 {Properties Value.이름[]: Text.Content}님.

Content 테스트 내용 입력

## 루틴 설정

하단 Every 15 minutes - Every day 09:00 시 설정 후 Save