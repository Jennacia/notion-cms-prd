# PRD: 개인 개발 블로그 (Notion CMS)

## 1. 프로젝트 개요

| 항목 | 내용 |
|------|------|
| **프로젝트명** | 개인 개발 블로그 |
| **목적** | Notion을 CMS로 활용한 개인 기술 블로그 |
| **핵심 가치** | Notion에서 글을 작성하면 웹사이트에 자동으로 반영되어, 비개발자도 콘텐츠 관리가 가능함 |

### 1.1 CMS 선택 이유

- **콘텐츠 작성 편의성**: Notion의 익숙한 에디터로 글 작성·수정
- **별도 관리자 UI 불필요**: Notion이 곧 CMS이므로 커스텀 백오피스 개발 비용 절감
- **비개발자 친화**: 마크다운/코드 없이도 글·카테고리·태그 관리 가능
- **버전·협업**: Notion 기본 기능으로 초안 관리 및 협업 가능

---

## 2. 목표 및 성공 기준

### 2.1 목표

- Notion 데이터베이스를 단일 소스로 하는 기술 블로그 운영
- 발행된 글만 웹에 노출하고, 목록·상세·카테고리·검색으로 탐색 가능하게 함
- 모바일·태블릿·데스크톱에서 동일한 품질의 읽기 경험 제공

### 2.2 성공 기준 (MVP)

- [ ] Notion API로 글 목록·상세·본문을 정상 조회
- [ ] `Status = 발행됨`인 글만 공개
- [ ] 홈·상세·카테고리 페이지 동작
- [ ] 카테고리 필터 및 검색 동작
- [ ] 주요 뷰포트에서 레이아웃 깨짐 없음
- [ ] Vercel 배포 후 프로덕션 URL에서 접근 가능

---

## 3. 사용자 및 사용 시나리오

### 3.1 사용자

| 역할 | 설명 |
|------|------|
| **콘텐츠 작성자** | 블로그 운영자 (본인). Notion에서 글 작성·발행 |
| **방문자** | 기술 블로그를 읽는 개발자·학습자 |

### 3.2 주요 시나리오

1. **글 발행**: 작성자가 Notion DB에 글을 작성하고 `Status`를 `발행됨`으로 변경 → 웹에 반영
2. **최신 글 탐색**: 방문자가 홈에서 최근 글 목록 확인
3. **글 읽기**: 목록에서 글 선택 → 상세 페이지에서 본문 읽기
4. **카테고리 탐색**: 특정 카테고리만 필터링해 관련 글 목록 확인
5. **검색**: 제목·요약·태그 등으로 키워드 검색

---

## 4. 주요 기능

### 4.1 기능 목록

| # | 기능 | 설명 | 우선순위 |
|---|------|------|----------|
| 1 | 글 목록 조회 | Notion 데이터베이스에서 블로그 글 목록 가져오기 | P0 |
| 2 | 글 상세 페이지 | 개별 글 메타데이터 + Notion 페이지 본문 렌더링 | P0 |
| 3 | 카테고리 필터링 | Category 기준 글 목록 필터 | P0 |
| 4 | 검색 | 제목·요약·태그 등 키워드 검색 | P1 |
| 5 | 반응형 디자인 | 모바일·태블릿·데스크톱 대응 | P0 |

### 4.2 기능 상세

#### 4.2.1 글 목록 조회

- Notion Database Query API로 글 목록 조회
- `Status = 발행됨`인 항목만 노출
- 정렬: `Published` 내림차순 (최신순)
- 노출 필드: Title, Category, Tags, Published, Summary, Slug

#### 4.2.2 글 상세 페이지

- URL: `/posts/[slug]` (Slug 기반)
- 메타: 제목, 카테고리, 태그, 발행일, 요약
- 본문: Notion 페이지 블록을 HTML/React로 변환해 표시

#### 4.2.3 카테고리 필터링

- Category(select) 값별 글 목록
- URL 예: `/categories/[category]` 또는 쿼리 `?category=`

#### 4.2.4 검색

- 클라이언트 또는 서버에서 제목·Summary·Tags 매칭
- MVP: 단순 문자열 포함 검색 (대소문자 무시 권장)

#### 4.2.5 반응형 디자인

- Tailwind CSS 브레이크포인트 활용
- 목록: 모바일 1열, 태블릿 2열, 데스크톱 3열 등 그리드 조정
- 상세: 가독성 위한 최대 너비·타이포그래피

---

## 5. 기술 스택

| 영역 | 기술 |
|------|------|
| **Frontend** | Next.js 15, TypeScript |
| **CMS** | Notion API (`@notionhq/client`) |
| **Styling** | Tailwind CSS, shadcn/ui |
| **Icons** | Lucide React |
| **Deployment** | Vercel |

### 5.1 아키텍처 개요

```
[Notion Database] ←→ [Notion API] ←→ [Next.js Server/API]
                                              ↓
                                    [Pages: Home / Post / Category]
                                              ↓
                                         [Vercel CDN]
```

- 데이터 페칭: Server Components 또는 Route Handlers에서 Notion API 호출
- 캐싱: `revalidate` 등 ISR/캐시 전략으로 API 호출 최소화 (구현 단계에서 결정)

---

## 6. Notion 데이터베이스 구조

블로그 글은 **단일 Notion 데이터베이스**로 관리한다.

| 속성명 | Notion 타입 | 설명 | 웹 사용 |
|--------|-------------|------|---------|
| **Title** | title | 글 제목 | 목록·상세 제목 |
| **Category** | select | 카테고리 | 필터·표시 |
| **Tags** | multi_select | 태그 | 표시·검색 |
| **Published** | date | 발행일 | 정렬·표시 |
| **Status** | select | `초안` / `발행됨` | 발행 여부 필터 |
| **Slug** | rich_text | URL 경로 (`/posts/{slug}`) | 라우팅 |
| **Summary** | rich_text | 글 요약 | 목록 카드·SEO |
| **Content** | page content | 본문 (연결된 페이지 블록) | 상세 본문 |

### 6.1 Status 규칙

- `초안`: 웹에 노출하지 않음
- `발행됨`: 목록·상세·검색·카테고리에 포함

### 6.2 Slug 규칙

- 영문 소문자, 숫자, 하이픈 권장 (예: `nextjs-notion-cms`)
- 고유값 유지 (중복 시 라우팅 충돌)

---

## 7. 화면 구성

### 7.1 사이트맵

```
/                          홈 (최근 글 목록)
/posts/[slug]              글 상세
/categories/[category]     카테고리별 글 목록
/search (선택)             검색 결과 (MVP에서 홈 내 검색 UI도 가능)
```

### 7.2 화면별 요구사항

#### 홈 (`/`)

- 최근 발행 글 목록 (카드 또는 리스트)
- 각 항목: 제목, 요약, 카테고리, 발행일, 태그(선택)
- 카테고리 필터 UI / 검색 입력 (MVP 범위에 포함)
- 글 클릭 시 `/posts/[slug]` 이동

#### 글 상세 (`/posts/[slug]`)

- 제목, 발행일, 카테고리, 태그
- Notion 본문 렌더링 (제목, 문단, 코드, 이미지 등)
- 목록으로 돌아가기 링크

#### 카테고리 (`/categories/[category]`)

- 해당 Category의 발행 글만 목록 표시
- 홈과 동일한 카드/리스트 패턴

### 7.3 UI/UX 가이드

- shadcn/ui 컴포넌트로 일관된 버튼·카드·입력 UI
- Lucide React 아이콘 (검색, 태그, 달력 등)
- 다크 모드: MVP 이후 검토 (비범위)

---

## 8. MVP 범위

### 8.1 포함 (In Scope)

- [x] Notion API 연동
- [x] 글 목록 페이지 (홈)
- [x] 글 상세 페이지
- [x] 카테고리 필터링
- [x] 검색 기능
- [x] 기본 스타일링 (Tailwind + shadcn/ui)
- [x] 반응형 디자인

### 8.2 제외 (Out of Scope, MVP 이후)

- 댓글 시스템
- 조회수·좋아요
- RSS / sitemap 자동 생성 (필요 시 Phase 2)
- 다국어
- 관리자 대시보드 (Notion이 CMS 역할)
- OAuth·회원 기능

---

## 9. 환경 변수

| 변수명 | 설명 | 노출 |
|--------|------|------|
| `NOTION_API_KEY` | Notion Integration Secret | 서버만 |
| `NOTION_DATABASE_ID` | 블로그 글 DB ID | 서버만 |

- `.env.local`에 저장, Vercel 프로젝트 환경 변수에 동일 설정
- 클라이언트 번들에 API 키 포함 금지

---

## 10. API 및 데이터 레이어 (구현 가이드)

### 10.1 권장 모듈 구조

```
lib/
  notion/
    client.ts      # Notion 클라이언트 싱글톤
    posts.ts       # getPosts, getPostBySlug, getPostsByCategory
    types.ts       # Post, Category 등 타입
```

### 10.2 핵심 함수 (예시)

- `getPublishedPosts()` — 발행 글 목록, Published DESC
- `getPostBySlug(slug)` — Slug로 단일 글 + page id
- `getPostContent(pageId)` — 블록 children 조회 및 변환
- `getPostsByCategory(category)` — Category 필터
- `searchPosts(query)` — 제목·Summary·Tags 검색

### 10.3 에러 처리

- Slug 미존재 → 404
- Notion API 실패 → 500 + 로깅, 사용자-facing 메시지 단순화

---

## 11. 구현 단계

| 단계 | 작업 | 산출물 |
|------|------|--------|
| **1** | `@notionhq/client` 설치, `.env.local` 및 Integration 설정 | 패키지, 환경 변수 |
| **2** | Notion DB 생성, 속성 정의, Integration 연결 | DB 스키마, 샘플 글 |
| **3** | 글 목록 API 함수 구현 (`getPublishedPosts` 등) | `lib/notion/posts.ts` |
| **4** | 홈 화면에 글 목록 표시 | `app/page.tsx` |
| **5** | 글 상세 페이지 (`/posts/[slug]`) | 상세 페이지, 본문 렌더러 |
| **6** | 검색 및 카테고리 필터링 | 필터 UI, 카테고리 라우트 |
| **7** | 반응형 UI 스타일링 | Tailwind/shadcn 적용, Vercel 배포 |

### 11.1 Phase 2 (선택)

- ISR/`revalidate` 튜닝
- OG 이미지·메타 태그 (SEO)
- `notion-to-md` 또는 `react-notion-x` 등 본문 렌더링 라이브러리 검토
- 태그별 아카이브 페이지

---

## 12. 비기능 요구사항

| 항목 | 요구사항 |
|------|----------|
| **성능** | LCP 가능한 범위 내 목록·상세 로드 (캐시 활용) |
| **보안** | Notion API 키 서버 전용, 환경 변수 관리 |
| **접근성** | 시맨틱 HTML, 키보드 포커스, 대비 (기본 수준) |
| **호환성** | 최신 Chrome, Safari, Firefox, Edge / iOS·Android 모바일 |

---

## 13. 리스크 및 대응

| 리스크 | 대응 |
|--------|------|
| Notion API Rate Limit | 캐싱, 배치 요청 최소화 |
| 본문 블록 타입 미지원 | 지원 블록 목록 문서화, fallback UI |
| Slug 중복·누락 | Notion DB 템플릿·검증 규칙 |
| Integration 권한 누락 | DB·페이지에 Integration 연결 체크리스트 |

---

## 14. 참고 자료

- [Notion API Documentation](https://developers.notion.com/)
- [@notionhq/client](https://www.npmjs.com/package/@notionhq/client)
- [Next.js 15 Documentation](https://nextjs.org/docs)
- [shadcn/ui](https://ui.shadcn.com/)
- [Vercel Deployment](https://vercel.com/docs)

---

## 15. 문서 이력

| 버전 | 날짜 | 변경 내용 |
|------|------|-----------|
| 0.1 | 2026-05-17 | 초안 작성 |
