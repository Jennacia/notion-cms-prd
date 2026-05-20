# 개인 개발 블로그 (Notion CMS) - 개발 로드맵

## 📋 개발 로드맵 개요

이 로드맵은 **올바른 개발 순서**를 따라 Notion CMS 기반 개인 블로그를 단계적으로 구축하는 계획입니다.
각 단계마다 필수 작업, 순서 근거, 예상 기간, 완료 기준을 명시했습니다.

---

## Phase 1️⃣: 프로젝트 초기 설정 (골격 구축)

### 목표
Next.js 프로젝트 생성, 기본 구조 설정, Notion API 연동 준비

### 왜 이 순서인가?
프로젝트의 기초 없이는 다른 모든 작업이 불가능합니다. 
개발 환경, 타입스크립트 설정, 환경 변수, Notion 연동이 완성되어야 실제 기능 개발을 시작할 수 있습니다.

### 작업 목록

#### 1.1 프로젝트 초기화
- [ ] Next.js 15 프로젝트 생성 (`create-next-app`)
- [ ] TypeScript 설정 (strict mode)
- [ ] ESLint, Prettier 설정
- [ ] git 초기화 및 .gitignore 설정

**예상 시간**: 20분

#### 1.2 의존성 설치
- [ ] Tailwind CSS 설치 및 설정
- [ ] shadcn/ui 초기화
- [ ] Lucide React 설치
- [ ] `@notionhq/client` 설치

```bash
npm install @notionhq/client
npx shadcn-ui@latest init
npm install lucide-react
```

**예상 시간**: 15분

#### 1.3 환경 변수 설정
- [ ] `.env.local` 파일 생성
- [ ] `NOTION_API_KEY` 추가
- [ ] `NOTION_DATABASE_ID` 추가
- [ ] Notion Integration 생성 (https://www.notion.so/my-integrations)

```env
NOTION_API_KEY=your_api_key_here
NOTION_DATABASE_ID=your_database_id_here
```

**예상 시간**: 10분

#### 1.4 기본 폴더 구조 생성
```
src/
  app/
    page.tsx              # 홈 페이지
    posts/
      [slug]/
        page.tsx          # 글 상세 페이지
    categories/
      [category]/
        page.tsx          # 카테고리 페이지
    layout.tsx            # 공통 레이아웃
  lib/
    notion/
      client.ts           # Notion 클라이언트
      posts.ts            # 글 관련 함수
      types.ts            # 타입 정의
  components/
    ...                   # 공통 컴포넌트
  styles/
    globals.css           # 전역 스타일
```

- [ ] 폴더 구조 생성
- [ ] 파일 템플릿 작성

**예상 시간**: 15분

#### 1.5 Notion 데이터베이스 셋업
- [ ] Notion DB 생성 (블로그 글 관리용)
- [ ] 필수 속성 설정:
  - Title (title)
  - Category (select)
  - Tags (multi_select)
  - Published (date)
  - Status (select: 초안, 발행됨)
  - Slug (rich_text)
  - Summary (rich_text)
  - Content (page content)
- [ ] Integration에 DB 접근 권한 부여
- [ ] 테스트 글 1~2개 작성

**예상 시간**: 20분

### ✅ Phase 1 완료 기준
- [ ] Next.js 프로젝트 정상 실행 (`npm run dev`)
- [ ] 모든 필수 패키지 설치됨
- [ ] `.env.local`에 Notion API 키 설정됨
- [ ] 기본 폴더 구조 생성됨
- [ ] Notion DB에 Integration 연결됨

### 📊 예상 소요 시간: **80분** (약 1.5시간)

---

## Phase 2️⃣: 공통 모듈/컴포넌트 개발

### 목표
Notion API 호출 로직, 타입 정의, 공통 컴포넌트 구현

### 왜 이 순서인가?
핵심 기능 개발을 시작하기 전에 **재사용 가능한 모듈과 컴포넌트**를 먼저 만들어야 합니다.
이를 통해 나중의 페이지 구현이 간결해지고, 코드 중복을 줄일 수 있습니다.

### 작업 목록

#### 2.1 Notion 클라이언트 설정
- [ ] `lib/notion/client.ts` 작성
  - Notion 클라이언트 싱글톤 생성
  - API 키 관리

```typescript
import { Client } from '@notionhq/client';

export const notion = new Client({
  auth: process.env.NOTION_API_KEY,
});
```

**예상 시간**: 10분

#### 2.2 타입 정의
- [ ] `lib/notion/types.ts` 작성
  - `Post` 인터페이스
  - `Category`, `Tag` 타입
  - Notion API 응답 타입 정의

```typescript
export interface Post {
  id: string;
  title: string;
  slug: string;
  category: string;
  tags: string[];
  published: Date;
  summary: string;
  status: 'draft' | 'published';
}
```

**예상 시간**: 15분

#### 2.3 Notion API 함수 구현
- [ ] `lib/notion/posts.ts` 작성
  - `getPublishedPosts()` — 발행 글 목록 조회 (최신순)
  - `getPostBySlug(slug)` — Slug로 단일 글 조회
  - `getPostsByCategory(category)` — 카테고리별 글 조회
  - `searchPosts(query)` — 검색 함수 (제목, 요약, 태그)
  - `getPostContent(pageId)` — 글 본문 블록 조회
  - `getAllCategories()` — 모든 카테고리 목록

**예상 시간**: 60분

#### 2.4 공통 컴포넌트 구현
- [ ] `components/Header.tsx` — 헤더 (로고, 네비게이션)
- [ ] `components/Footer.tsx` — 푸터
- [ ] `components/PostCard.tsx` — 글 카드 (목록 표시용)
- [ ] `components/CategoryFilter.tsx` — 카테고리 필터 UI
- [ ] `components/SearchInput.tsx` — 검색 입력 필드

**예상 시간**: 45분

#### 2.5 레이아웃 및 스타일
- [ ] `app/layout.tsx` 작성
  - Header, Footer 통합
  - 전역 스타일 적용
  - 메타 정보 설정
- [ ] `styles/globals.css` 커스터마이즈
  - Tailwind 기본 설정
  - 글꼴, 색상 정의

**예상 시간**: 20분

### ✅ Phase 2 완료 기준
- [ ] Notion API 함수 구현 완료 및 테스트됨
- [ ] 타입 정의가 TypeScript strict mode 통과
- [ ] 공통 컴포넌트 작성 완료
- [ ] 레이아웃 및 기본 스타일 적용됨

### 📊 예상 소요 시간: **150분** (약 2.5시간)

---

## Phase 3️⃣: 핵심 기능 개발

### 목표
블로그의 핵심 페이지 구현 (글 목록, 글 상세, 카테고리)

### 왜 이 순서인가?
공통 모듈과 컴포넌트가 준비되었으므로, 이제 실제 **사용자가 보는 페이지**를 구현할 수 있습니다.
이 단계가 완료되면 기본적인 블로그 기능이 동작합니다.

### 작업 목록

#### 3.1 홈 페이지 (`/`)
- [ ] `app/page.tsx` 구현
  - 최근 발행 글 목록 표시
  - PostCard 컴포넌트로 렌더링
  - 클릭 시 상세 페이지로 이동
  - 카테고리 필터 UI 포함
  - 모바일, 태블릿, 데스크톱 반응형 레이아웃

```typescript
// 글 목록을 3열 그리드로 표시 (데스크톱)
// 2열 (태블릿), 1열 (모바일)
export default async function Home() {
  const posts = await getPublishedPosts();
  return (
    <div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
      {posts.map(post => <PostCard key={post.id} post={post} />)}
    </div>
  );
}
```

**예상 시간**: 30분

#### 3.2 글 상세 페이지 (`/posts/[slug]`)
- [ ] `app/posts/[slug]/page.tsx` 구현
  - 글 메타 정보 (제목, 발행일, 카테고리, 태그) 표시
  - Notion 본문 렌더링
  - 뒤로가기 링크
- [ ] Notion 블록을 React 컴포넌트로 변환
  - Paragraph, Heading 1-3, Image, Code, Quote, List 등
  - `notion-to-md` 또는 직접 구현

**예상 시간**: 90분

#### 3.3 카테고리 페이지 (`/categories/[category]`)
- [ ] `app/categories/[category]/page.tsx` 구현
  - 해당 카테고리의 발행 글만 목록 표시
  - 홈과 동일한 레이아웃
  - 카테고리 제목 표시
- [ ] 동적 라우트 생성
  - `generateStaticParams()` 으로 사전 생성

**예상 시간**: 30분

#### 3.4 검색 기능 (클라이언트 사이드)
- [ ] 홈 페이지에 검색 입력 UI 통합
- [ ] 클라이언트 JavaScript로 검색 로직 구현
  - 제목, 요약, 태그 포함 여부 확인
  - 대소문자 무시 검색
  - 실시간 필터링

**예상 시간**: 30분

#### 3.5 404 페이지
- [ ] `app/not-found.tsx` 작성
  - Slug가 없는 경우
  - 사용자 친화적 메시지 + 홈 링크

**예상 시간**: 10분

### ✅ Phase 3 완료 기준
- [ ] 홈 페이지에서 최근 글 목록 표시됨
- [ ] 글 상세 페이지에서 Notion 본문이 제대로 렌더링됨
- [ ] 카테고리 필터링이 정상 동작
- [ ] 검색 기능이 정상 동작
- [ ] 모든 주요 페이지에서 404 에러 없음

### 📊 예상 소요 시간: **190분** (약 3시간)

---

## Phase 4️⃣: 추가 기능 개발

### 목표
사용자 경험을 향상시키는 추가 기능 구현

### 왜 이 순서인가?
핵심 기능이 안정적으로 동작한 후, 접근성과 사용성을 개선합니다.
이 단계를 통해 블로그가 더 완성된 느낌을 갖추게 됩니다.

### 작업 목록

#### 4.1 SEO 최적화
- [ ] 각 페이지에 메타 태그 추가 (`metadata` 객체)
  - 제목, 설명, OG 이미지
  - 카테고리, 글 상세 페이지 등
- [ ] `robots.txt`, `sitemap.xml` 생성 (정적 또는 동적)

**예상 시간**: 30분

#### 4.2 접근성 개선
- [ ] 시맨틱 HTML 검토
- [ ] 키보드 네비게이션 테스트
- [ ] 색상 대비 확인
- [ ] alt 텍스트 추가 (이미지)

**예상 시간**: 20분

#### 4.3 검색 고도화 (선택)
- [ ] 서버 사이드 검색 API 구현
  - Route Handler (`app/api/search/route.ts`)
  - 쿼리 기반 검색
- [ ] 검색 결과 페이지 (`/search?q=...`)

**예상 시간**: 40분 (선택)

#### 4.4 카테고리/태그 아카이브 (선택)
- [ ] `/tags/[tag]` 페이지 추가
- [ ] 태그별 글 목록 표시

**예상 시간**: 30분 (선택)

#### 4.5 다크 모드 지원 (선택)
- [ ] Tailwind dark mode 설정
- [ ] 토글 UI 컴포넌트
- [ ] localStorage에 사용자 선택 저장

**예상 시간**: 40분 (선택)

### ✅ Phase 4 완료 기준
- [ ] 각 페이지에 기본 메타 태그 추가됨
- [ ] 접근성 기본 요구사항 충족
- [ ] 선택 기능 1~2개 구현됨 (시간/우선순위에 따라)

### 📊 예상 소요 시간: **120분** (약 2시간, 선택 기능 제외)

---

## Phase 5️⃣: 최적화 및 배포

### 목표
성능 최적화, 최종 테스트, Vercel 배포

### 왜 이 순서인가?
**모든 기능이 완성된 후**에 성능 최적화와 배포를 수행합니다.
미리 최적화하면 나중에 기능 추가 시 다시 해야 할 수 있으므로, 기능 완성 후 최적화하는 것이 효율적입니다.

### 작업 목록

#### 5.1 성능 최적화
- [ ] Next.js Image 컴포넌트 사용
  - Notion 이미지 최적화
- [ ] ISR (Incremental Static Regeneration) 설정
  - `revalidate` 옵션 (글 목록: 1시간, 상세: 24시간 등)
- [ ] 캐싱 전략 수립
  - API 호출 최소화

```typescript
export const revalidate = 3600; // 1시간마다 재생성
```

**예상 시간**: 30분

#### 5.2 번들 최적화
- [ ] 불필요한 의존성 제거
- [ ] Dynamic import로 코드 분할
- [ ] Tree-shaking 확인

**예상 시간**: 20분

#### 5.3 최종 테스트
- [ ] 기능 테스트
  - 글 목록 로드, 상세 페이지 열기, 검색, 필터링
- [ ] 반응형 테스트
  - 모바일 (375px), 태블릿 (768px), 데스크톱 (1024px+)
- [ ] 크로스 브라우저 테스트
  - Chrome, Safari, Firefox, Edge
- [ ] 성능 측정
  - Lighthouse 점수 확인
  - Core Web Vitals

**예상 시간**: 40분

#### 5.4 Vercel 배포 준비
- [ ] GitHub 저장소 생성
- [ ] 로컬 커밋 및 푸시
- [ ] Vercel 프로젝트 연결
- [ ] 환경 변수 설정 (Vercel Dashboard)
  - `NOTION_API_KEY`
  - `NOTION_DATABASE_ID`

**예상 시간**: 20분

#### 5.5 배포 및 검증
- [ ] Vercel에서 자동 배포 확인
- [ ] 프로덕션 URL에서 기능 검증
- [ ] 에러 로깅 확인 (Vercel Analytics)

**예상 시간**: 15분

#### 5.6 배포 후 체크리스트
- [ ] 모든 페이지 404 에러 없음
- [ ] 이미지/아이콘 정상 표시
- [ ] 검색/필터 기능 동작
- [ ] 모바일에서 정상 표시

**예상 시간**: 15분

### ✅ Phase 5 완료 기준
- [ ] Lighthouse 성능 점수 70+ 이상
- [ ] 모든 기능이 프로덕션에서 정상 동작
- [ ] 반응형 레이아웃 모든 뷰포트에서 정상 표시
- [ ] 환경 변수 Vercel에 설정됨
- [ ] 프로덕션 URL에서 접근 가능

### 📊 예상 소요 시간: **140분** (약 2.3시간)

---

## 📊 전체 개발 일정

| Phase | 단계명 | 예상 시간 | 누적 시간 |
|-------|--------|---------|---------|
| 1 | 프로젝트 초기 설정 | 80분 | 80분 |
| 2 | 공통 모듈/컴포넌트 | 150분 | 230분 |
| 3 | 핵심 기능 개발 | 190분 | 420분 |
| 4 | 추가 기능 개발 | 120분 | 540분 |
| 5 | 최적화 및 배포 | 140분 | 680분 |
| **총합** | | **680분** | **약 11.3시간** |

### ⏱️ 권장 일정
- **단기**: 3일 (매일 4시간 투자)
- **여유있게**: 5~7일 (매일 2~3시간 투자)
- **업무 병행**: 2주 (하루 1시간 투자)

---

## 🎯 주요 마일스톤

| 마일스톤 | 달성 조건 | Phase |
|---------|----------|-------|
| **✅ 기초 완성** | 환경 설정 완료, Notion API 연동 | Phase 1-2 |
| **✅ MVP 완성** | 글 목록, 상세, 카테고리, 검색 동작 | Phase 3 |
| **✅ 사용자 경험 향상** | SEO, 접근성, 추가 기능 | Phase 4 |
| **✅ 프로덕션 준비** | 성능 최적화, 배포 테스트 | Phase 5 |
| **✅ 라이브** | Vercel 배포 완료, 공개 | Phase 5 |

---

## 🚀 다음 단계

### MVP 이후 선택 사항 (Phase 6+)

다음과 같은 기능은 MVP 이후에 추가할 수 있습니다:

- [ ] **댓글 시스템** (Disqus, Giscus 등)
- [ ] **조회수/좋아요** (Database 추가)
- [ ] **RSS 피드** 자동 생성
- [ ] **다국어 지원** (i18n)
- [ ] **Notion 공식 데이터베이스 미리보기** (빠른 미리보기)
- [ ] **성능 분석** (Google Analytics)
- [ ] **백업 및 아카이빙** 시스템

---

## 📝 개발 시 주의사항

### 환경 변수 관리
```env
# .env.local (로컬 개발)
NOTION_API_KEY=secret_...
NOTION_DATABASE_ID=550e...

# Vercel 환경 변수는 따로 설정
# (Dashboard → Settings → Environment Variables)
```

### Notion API Rate Limit
- 공식 문서: 최대 3 요청/초
- 캐싱으로 API 호출 최소화
- ISR 설정으로 불필요한 재생성 방지

### 타입 안정성
- 항상 TypeScript `strict: true` 유지
- Notion API 응답을 타입 검증

### 깃 커밋 전략
각 Phase 완료 후 커밋:
```bash
git commit -m "Phase 1: 프로젝트 초기 설정"
git commit -m "Phase 2: 공통 모듈/컴포넌트 개발"
git commit -m "Phase 3: 핵심 기능 개발"
git commit -m "Phase 4: 추가 기능 개발"
git commit -m "Phase 5: 최적화 및 배포"
```

---

## 📚 참고 자료

- [Notion API 공식 문서](https://developers.notion.com/)
- [Next.js 15 공식 문서](https://nextjs.org/docs)
- [Tailwind CSS](https://tailwindcss.com/)
- [shadcn/ui 컴포넌트](https://ui.shadcn.com/)
- [Vercel 배포 가이드](https://vercel.com/docs)
- [notion-to-md 라이브러리](https://github.com/souvikhaldar/notion-to-md)
- [react-notion-x 라이브러리](https://github.com/NotionX/react-notion-x)

---

## ✍️ 문서 정보

- **작성일**: 2026-05-20
- **기반 문서**: docs/PRD.md
- **최종 수정 예정**: Phase 5 완료 후

---

**행운을 빕니다! 🚀**
