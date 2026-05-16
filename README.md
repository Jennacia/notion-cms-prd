# 개인 개발 블로그 - Notion CMS

Notion을 콘텐츠 관리 시스템(CMS)으로 활용한 개인 개발 블로그 플랫폼입니다. Notion에서 글을 작성하면 자동으로 웹사이트에 반영되어, 별도의 관리자 UI 없이도 비개발자가 콘텐츠를 관리할 수 있습니다.

## 📖 프로젝트 개요

| 항목 | 내용 |
|------|------|
| **프로젝트명** | 개인 개발 블로그 |
| **목적** | Notion을 CMS로 활용한 기술 블로그 |
| **핵심 가치** | Notion에서의 글 작성이 웹사이트에 자동으로 반영되는 통합 콘텐츠 관리 |

### 주요 특징

- 🚀 **Notion 통합**: Notion API를 통한 자동 데이터 동기화
- ✏️ **쉬운 콘텐츠 관리**: Notion의 직관적인 에디터로 글 작성·수정
- 🎨 **반응형 디자인**: 모바일·태블릿·데스크톱 모두 최적화
- 🔍 **검색 및 필터**: 카테고리 필터링과 키워드 검색 지원
- 📱 **모던 UI**: shadcn/ui와 Tailwind CSS로 구현된 깔끔한 디자인

---

## 🚀 설치 방법

### 필수 조건

- Node.js 18 이상
- npm 또는 yarn
- Notion 계정

### 1단계: 프로젝트 클론 및 패키지 설치

```bash
git clone <repository-url>
cd notion-cms-prd
npm install
# 또는 yarn install
```

### 2단계: Notion Integration 설정

1. [Notion Developers](https://www.notion.so/my-integrations)에서 새 Integration 생성
2. 생성된 Integration의 **Secret 토큰** 복사
3. Integration을 블로그 데이터베이스에 초대

### 3단계: 환경 변수 설정

프로젝트 루트에 `.env.local` 파일 생성 후 다음 정보 입력:

```env
NOTION_API_KEY=your_notion_api_key_here
NOTION_DATABASE_ID=your_notion_database_id_here
```

- `NOTION_API_KEY`: Notion Integration의 Secret 토큰
- `NOTION_DATABASE_ID`: 블로그 글을 관리할 Notion 데이터베이스 ID

### 4단계: 로컬 개발 서버 실행

```bash
npm run dev
# 또는 yarn dev
```

`http://localhost:3000`에서 개발 서버가 실행됩니다.

### 5단계: 프로덕션 빌드 및 배포

```bash
npm run build
npm start
```

---

## ✨ 주요 기능

| # | 기능 | 설명 |
|---|------|------|
| 1 | **글 목록 조회** | Notion 데이터베이스에서 블로그 글 목록 자동 조회 |
| 2 | **글 상세 페이지** | 개별 글의 메타데이터와 Notion 본문 렌더링 |
| 3 | **카테고리 필터링** | 카테고리별 글 목록 필터링 |
| 4 | **검색 기능** | 제목·요약·태그 키워드 검색 |
| 5 | **반응형 디자인** | 모바일·태블릿·데스크톱 모두 최적화 |

### 페이지 구조

```
/                    → 홈 (최근 글 목록)
/posts/[slug]        → 글 상세 페이지
/categories/[cat]    → 카테고리별 글 목록
```

---

## 🛠 기술 스택

### Frontend & Framework
- **Next.js 15** - React 기반 풀스택 프레임워크
- **TypeScript** - 타입 안전성을 위한 정적 타입 언어
- **React 19** - UI 컴포넌트 라이브러리

### 스타일링 & UI
- **Tailwind CSS** - 유틸리티 기반 CSS 프레임워크
- **shadcn/ui** - 고급 UI 컴포넌트 라이브러리
- **Lucide React** - 아이콘 라이브러리

### CMS & API
- **Notion API** (`@notionhq/client`) - Notion 데이터 연동

### 배포
- **Vercel** - Next.js 최적화된 호스팅 플랫폼

---

## 📋 Notion 데이터베이스 구조

블로그 글은 다음 속성으로 구성된 단일 Notion 데이터베이스로 관리됩니다:

| 속성명 | Notion 타입 | 설명 |
|--------|-------------|------|
| **Title** | title | 글 제목 |
| **Category** | select | 카테고리 분류 |
| **Tags** | multi_select | 복수 태그 |
| **Published** | date | 발행일 |
| **Status** | select | 상태 (`초안` / `발행됨`) |
| **Slug** | rich_text | URL 경로 (`/posts/{slug}`) |
| **Summary** | rich_text | 글 요약 |
| **Content** | page content | 본문 (연결된 페이지 블록) |

### Status 규칙
- `초안`: 웹에 노출하지 않음
- `발행됨`: 목록·상세·검색에 포함

---

## 📚 PRD 문서

전체 프로젝트 요구사항과 상세 설계는 다음 문서를 참고하세요:

📄 **[PRD: 개인 개발 블로그 (Notion CMS)](./docs/PRD.md)**

- 목표 및 성공 기준
- 사용자 및 사용 시나리오
- 화면 구성 및 UI/UX 가이드
- MVP 범위
- 구현 단계
- 비기능 요구사항 및 리스크 대응

---

## 🔧 개발 스크립트

```bash
# 개발 서버 실행
npm run dev

# 타입 체크
npm run type-check

# 프로덕션 빌드
npm run build

# 프로덕션 서버 실행
npm start

# Lint (코드 스타일 검사)
npm run lint
```

---

## 🌍 배포

### Vercel을 통한 배포

1. [Vercel](https://vercel.com)에 프로젝트 연동
2. 환경 변수 설정
   - `NOTION_API_KEY`
   - `NOTION_DATABASE_ID`
3. 자동 배포 (git push 시 자동 배포)

---

## 📋 MVP 범위

### 포함 항목
- ✅ Notion API 연동
- ✅ 글 목록 페이지 (홈)
- ✅ 글 상세 페이지
- ✅ 카테고리 필터링
- ✅ 검색 기능
- ✅ 기본 스타일링 (Tailwind + shadcn/ui)
- ✅ 반응형 디자인

### Phase 2 (향후 계획)
- 댓글 시스템
- 조회수·좋아요
- RSS / sitemap 자동 생성
- 다크 모드
- 태그별 아카이브 페이지

---

## 🔐 보안 주의사항

- **API 키 보호**: `NOTION_API_KEY`는 `.env.local`에 저장되며 서버 전용
- **클라이언트 번들**: API 키는 클라이언트 번들에 포함되지 않음
- **.env.local 무시**: `.gitignore`에 추가되어 git에 커밋되지 않음

---

## 📞 지원 및 문의

문제가 발생하거나 기능 제안이 있는 경우:

1. [GitHub Issues](https://github.com/yourusername/notion-cms-prd/issues)에서 이슈 생성
2. PRD 문서의 "리스크 및 대응" 섹션 참고

---

## 📖 참고 자료

- [Notion API 문서](https://developers.notion.com/)
- [@notionhq/client NPM](https://www.npmjs.com/package/@notionhq/client)
- [Next.js 15 문서](https://nextjs.org/docs)
- [shadcn/ui](https://ui.shadcn.com/)
- [Tailwind CSS](https://tailwindcss.com/)
- [Vercel 배포 가이드](https://vercel.com/docs)

---

## 📝 라이선스

MIT License

---

**마지막 수정**: 2026-05-17
