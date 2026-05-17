# 📚 개인 개발 블로그

**Notion을 CMS로 활용한 모던 개인 기술 블로그 플랫폼**

> Notion에서 글을 작성하면 자동으로 블로그에 반영되는 동적 블로그 시스템

---

## 🎯 프로젝트 소개

이 프로젝트는 Notion을 CMS(콘텐츠 관리 시스템)로 활용하여 개인 기술 블로그를 구축합니다. 
복잡한 데이터베이스 관리 없이 Notion에서 직관적으로 글을 작성하면, 자동으로 블로그에 반영되는 구조입니다.

### ✨ 주요 특징

- 🚀 **빠른 성능**: Next.js 15 + Turbopack으로 극대화된 개발/빌드 속도
- 📝 **쉬운 콘텐츠 관리**: Notion API를 통한 직관적 글 관리
- 🎨 **모던 UI**: Tailwind CSS + shadcn/ui 기반의 세련된 디자인
- 📱 **반응형 디자인**: 모바일, 태블릿, 데스크톱 완벽 지원
- 🔍 **검색 & 필터링**: 글 검색 및 카테고리별 필터링
- ⚡ **Server Actions**: Next.js 최신 기술 활용

---

## 🛠️ 기술 스택

### Frontend
- **Framework**: Next.js 15.5.3 (App Router)
- **Runtime**: React 19.1.0
- **Language**: TypeScript 5
- **Styling**: Tailwind CSS v4 + shadcn/ui
- **Icons**: Lucide React
- **Forms**: React Hook Form + Zod

### CMS & Backend
- **CMS**: Notion API (@notionhq/client)
- **Data Fetching**: Server Actions

### Development
- **Linter**: ESLint
- **Formatter**: Prettier
- **Git Hooks**: Husky + lint-staged

### Deployment
- **Hosting**: Vercel
- **Environment**: Node.js

---

## 📊 Notion 데이터베이스 구조

Notion에서 다음과 같은 구조의 데이터베이스를 생성합니다:

| 필드 | 타입 | 설명 |
|------|------|------|
| Title | Title | 글의 제목 |
| Category | Select | 글의 카테고리 |
| Tags | Multi-select | 검색용 태그 |
| Published | Date | 발행 날짜 |
| Status | Select | Draft / Published |
| Content | Page | 글의 본문 |
| Slug | Text | URL-friendly 식별자 |

---

## 🚀 빠른 시작

### 1. 저장소 클론

```bash
git clone https://github.com/yourusername/notion-cms-blog.git
cd notion-cms-blog
```

### 2. 의존성 설치

```bash
npm install
```

### 3. 환경 변수 설정

`.env.local` 파일을 생성하고 다음 환경 변수를 설정합니다:

```env
# Notion API 설정
NEXT_PUBLIC_NOTION_DATABASE_ID=your_database_id_here
NOTION_API_TOKEN=secret_your_api_token_here
```

**Notion API 키 발급 방법:**
1. [Notion Developers](https://www.notion.so/my-integrations)에서 새로운 Integration 생성
2. Capability에서 `Read content` 권한 선택
3. 생성된 Internal Integration Token 복사
4. Notion 페이지에서 Integration 권한 부여

### 4. 개발 서버 실행

```bash
npm run dev
```

브라우저에서 [http://localhost:3000](http://localhost:3000)을 열어 확인합니다.

---

## 📋 사용 가능한 명령어

```bash
# 개발 서버 실행 (Turbopack)
npm run dev

# 프로덕션 빌드
npm run build

# 프로덕션 서버 실행
npm run start

# 모든 검사 실행 (린트, 타입 체크, 포매팅)
npm run check-all

# ESLint 실행
npm run lint

# Prettier 포매팅
npm run format

# 타입 체크
npm run type-check

# shadcn/ui 컴포넌트 추가
npx shadcn-ui@latest add [component-name]
```

---

## 📁 프로젝트 구조

```
notion-cms-blog/
├── app/                      # Next.js App Router
│   ├── (home)/              # 홈 페이지
│   ├── posts/               # 글 관련 페이지
│   │   └── [slug]/          # 글 상세 페이지 (동적)
│   └── layout.tsx           # 루트 레이아웃
├── components/              # React 컴포넌트
│   ├── ui/                  # shadcn/ui 컴포넌트
│   ├── layout/              # 레이아웃 컴포넌트 (Header, Footer 등)
│   └── posts/               # 글 관련 컴포넌트
├── lib/                     # 유틸리티 함수
│   ├── notion.ts            # Notion API 클라이언트
│   └── utils.ts             # 기타 유틸리티
├── public/                  # 정적 자산
├── docs/                    # 개발 문서
│   ├── PRD.md              # 프로젝트 요구사항
│   ├── ROADMAP.md          # 개발 로드맵
│   └── guides/             # 개발 가이드
├── .env.local              # 환경 변수 (커밋 금지)
├── next.config.ts          # Next.js 설정
├── tsconfig.json           # TypeScript 설정
└── package.json            # 프로젝트 메타데이터
```

---

## 🎨 주요 기능

### 1️⃣ 글 목록 페이지 (`/`)
- Notion 데이터베이스의 모든 발행된 글 표시
- 최신 글부터 역순 정렬
- 카테고리별 필터링
- 검색 기능

### 2️⃣ 글 상세 페이지 (`/posts/[slug]`)
- Notion 페이지의 전체 콘텐츠 렌더링
- 메타데이터 표시 (작성일, 카테고리, 태그)
- 이전/다음 글 네비게이션

### 3️⃣ 카테고리 필터링
- 사이드바에서 카테고리 선택
- 선택된 카테고리의 글만 표시

### 4️⃣ 검색 기능
- 제목 및 태그 기반 실시간 검색

---

## 🔧 개발 가이드

자세한 개발 가이드는 `docs/` 디렉토리를 참고하세요:

- 📋 [프로젝트 요구사항 (PRD)](./docs/PRD.md)
- 🗺️ [개발 로드맵](./docs/ROADMAP.md)
- 📁 [프로젝트 구조](./docs/guides/project-structure.md)
- 🎨 [스타일링 가이드](./docs/guides/styling-guide.md)
- 🧩 [컴포넌트 패턴](./docs/guides/component-patterns.md)
- ⚡ [Next.js 15 전문 가이드](./docs/guides/nextjs-15.md)
- 📝 [폼 처리 완전 가이드](./docs/guides/forms-react-hook-form.md)

---

## 🚀 배포

이 프로젝트는 Vercel에서 호스팅하도록 최적화되어 있습니다.

### Vercel 배포

1. [Vercel](https://vercel.com)에 GitHub 계정으로 로그인
2. "New Project" 클릭
3. 저장소 선택
4. 환경 변수 설정:
   - `NEXT_PUBLIC_NOTION_DATABASE_ID`
   - `NOTION_API_TOKEN`
5. 배포 클릭

### 커스텀 도메인 설정

Vercel 프로젝트 설정에서 "Domains" 탭에서 커스텀 도메인 추가 가능

---

## ✅ 완성도 체크리스트

프로젝트 완성 전 확인 사항:

```bash
npm run check-all   # 모든 검사 통과 확인
npm run build       # 빌드 성공 확인
```

- [ ] 타입 체크 통과
- [ ] ESLint 경고/에러 없음
- [ ] Prettier 포매팅 일관성
- [ ] 모든 환경에서 빌드 성공
- [ ] 반응형 디자인 테스트 완료
- [ ] Notion API 연동 테스트 완료

---

## 📚 참고 자료

### 공식 문서
- [Notion API 문서](https://developers.notion.com/)
- [Next.js 15 문서](https://nextjs.org/docs)
- [React 19 문서](https://react.dev/)
- [Tailwind CSS 문서](https://tailwindcss.com/docs)
- [shadcn/ui](https://ui.shadcn.com/)

### 유용한 링크
- [Next.js Examples](https://github.com/vercel/next.js/tree/canary/examples)
- [Notion API Examples](https://github.com/makenotion/notion-sdk-js/tree/main/examples)

---

## 🤝 기여하기

버그 리포트 및 기능 제안은 GitHub Issues에서 제시할 수 있습니다.

---

## 📄 라이선스

MIT License - 자유롭게 사용, 수정, 배포 가능합니다.

---

## 👨‍💻 개발자

**개인 프로젝트**

- 📧 Email: thinkjhjgjj@gmail.com
- 🔗 GitHub: [github.com/yourusername](https://github.com/yourusername)

---

**마지막 업데이트**: 2026년 5월 18일
