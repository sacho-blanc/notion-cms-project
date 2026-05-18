# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

---

## 📌 Project Overview

**Notion CMS 블로그** — Notion을 콘텐츠 관리 시스템(CMS)으로 활용한 개인 기술 블로그 플랫폼입니다.
Notion 데이터베이스에서 글을 작성하면 Next.js를 통해 자동으로 블로그에 반영되는 동적 시스템입니다.

**현재 진행 상황**: Phase 1 (프로젝트 기본 구조 및 UI 기초 완료)

### 📚 Project Documentation

- **PRD (Product Requirements)**: `docs/PRD.md` — 전체 요구사항 및 기능 명세
- **ROADMAP**: `docs/ROADMAP.md` — 개발 단계별 계획 (Phase 1-5)
- **Development Guides**: `docs/guides/` — 스타일링, 컴포넌트 패턴, Next.js 가이드

---

## 🛠️ Tech Stack

| 레이어            | 기술                                                  |
| ----------------- | ----------------------------------------------------- |
| **Framework**     | Next.js 15.5.3 (App Router + Turbopack)               |
| **Runtime**       | React 19.1.0 + TypeScript 5                           |
| **Styling**       | Tailwind CSS v4 + shadcn/ui (new-york)                |
| **Forms**         | React Hook Form + Zod                                 |
| **CMS**           | Notion API (@notionhq/client — Phase 2에서 추가 예정) |
| **Data Fetching** | Next.js Server Actions                                |
| **Development**   | ESLint + Prettier + Husky + lint-staged               |

---

## 🏗️ Project Architecture

### Directory Structure

```
src/
├── app/                          # Next.js App Router pages
│   ├── layout.tsx               # 루트 레이아웃 (테마, 글로벌 스타일)
│   ├── page.tsx                 # 홈 페이지 (글 목록 예정)
│   ├── login/
│   │   └── page.tsx             # 로그인 페이지
│   ├── signup/
│   │   └── page.tsx             # 회원가입 페이지
│   └── posts/
│       └── [slug]/
│           └── page.tsx         # 글 상세 페이지 (동적 라우트)
├── components/
│   ├── ui/                      # shadcn/ui 컴포넌트 (auto-generated)
│   ├── layout/
│   │   ├── container.tsx        # 컨테이너 래퍼
│   │   ├── header.tsx           # 사이트 헤더 + 네비게이션
│   │   └── footer.tsx           # 사이트 푸터
│   ├── navigation/
│   │   ├── main-nav.tsx         # 데스크톱 네비게이션
│   │   └── mobile-nav.tsx       # 모바일 네비게이션
│   ├── sections/
│   │   ├── hero.tsx             # 히어로 섹션
│   │   ├── features.tsx         # 기능 소개 섹션
│   │   └── cta.tsx              # Call-to-Action 섹션
│   ├── providers/
│   │   └── theme-provider.tsx   # 다크모드 테마 Provider
│   ├── login-form.tsx           # 로그인 폼
│   ├── signup-form.tsx          # 회원가입 폼
│   └── theme-toggle.tsx         # 다크모드 토글 버튼
└── lib/
    ├── env.ts                   # 환경 변수 타입 정의
    └── utils.ts                 # 유틸리티 함수들
```

### Core Architecture Pattern

#### 1. Component Organization

- **UI Components** (`components/ui/`): shadcn/ui 기반 저수준 컴포넌트 (수정 금지)
- **Layout Components** (`components/layout/`): 페이지 구조 (Header, Footer, Container)
- **Navigation Components** (`components/navigation/`): 반응형 네비게이션
- **Section Components** (`components/sections/`): 페이지 섹션 (Hero, Features, CTA)
- **Form Components** (`components/*.tsx`): React Hook Form + Zod 기반 타입 안전 폼
- **Providers** (`components/providers/`): React Context Providers (Theme, Auth 등)

#### 2. Notion API Integration (Phase 2 이후)

- **Location**: `src/lib/notion.ts` (향후 구현)
- **Pattern**: Server Actions를 통한 서버 사이드 Notion API 호출
- **Database Schema**: Title, Category, Tags, Published (Date), Status, Content, Slug
- **Key Functions** (Phase 2에서 구현):
  - `fetchPages()` — Published 글 목록 조회 (최신순)
  - `fetchPageContent()` — slug 기반 글 상세 조회
  - `fetchCategories()` — 카테고리 목록 조회

#### 3. Data Flow Architecture

```
┌─────────────────────────────────────────────────────┐
│                  Notion Database                    │
│  (title, category, tags, published, status, slug)  │
└────────────────────┬────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────┐
│           Server Actions (src/lib/)                 │
│     (Notion API 호출, 환경변수 보호)                  │
└────────────────────┬────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────┐
│        React Components (src/components/)           │
│  (Props 기반 UI 컴포넌트, 클라이언트 상호작용)         │
└────────────────────┬────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────┐
│            HTML/CSS (Tailwind CSS)                  │
│         (브라우저에서 렌더링되는 최종 결과)            │
└─────────────────────────────────────────────────────┘
```

#### 4. TypeScript Path Aliases

- `@/*` → `./src/*` (tsconfig.json에 정의)
- 항상 `@/components`, `@/lib` 등으로 import (상대 경로 X)

---

## ⚡ Commands

### 개발 서버 실행

```bash
npm run dev              # 개발 서버 실행 (Turbopack 사용, http://localhost:3000)
npm run build            # 프로덕션 빌드 (검증 목적)
npm run start            # 프로덕션 서버 실행 (로컬 검증)
```

### 코드 품질 검사 & 포매팅

```bash
npm run typecheck        # TypeScript 타입 검사
npm run lint             # ESLint 린트 검사
npm run lint:fix         # ESLint 자동 수정
npm run format           # Prettier 포매팅
npm run format:check     # Prettier 검사만 (수정 X)
npm run check-all        # 통합 검사 (타입체크 + 린트 + 포매팅 검사)
```

### UI 컴포넌트 추가

```bash
npx shadcn@latest add [component-name]    # shadcn/ui 컴포넌트 추가
npx shadcn@latest add button              # 예: Button 컴포넌트 추가
```

### Pre-commit Hooks (자동 실행)

- Husky + lint-staged가 자동으로 수행
- 커밋 전에 `*.{js,jsx,ts,tsx}` 파일: eslint --fix + prettier --write
- 커밋 전에 `*.{json,css,md}` 파일: prettier --write

---

## 🚀 초기 설정 (새로운 개발자용)

### 1단계: 저장소 클론 및 의존성 설치

```bash
git clone <repository-url>
cd notion-cms-project
npm install
```

### 2단계: 환경 변수 설정 (필요 시)

`.env.local` 파일을 프로젝트 루트에 생성:

```env
# Notion API (Phase 2 이후 필요)
NEXT_PUBLIC_NOTION_DATABASE_ID=your_database_id_here
NOTION_API_TOKEN=your_secret_api_token_here
```

**주의**:

- `NOTION_API_TOKEN`은 민감한 정보이므로 `.gitignore`에 포함됨
- `NEXT_PUBLIC_*` 접두사는 클라이언트에서 접근 가능 (민감정보 X)
- 실제 값은 프로젝트 리더에게 별도로 공유 받음

### 3단계: 코드 품질 검사 확인

```bash
npm run check-all        # 모든 검사가 통과하는지 확인
```

---

## 📋 개발 워크플로우

### 작업 시작 전

1. `npm run check-all` 실행하여 현재 상태 확인
2. 해당 Phase의 체크리스트 확인 (`docs/ROADMAP.md` 참고)
3. feature 브랜치 생성: `git checkout -b feature/feature-name`

### 개발 중

```bash
npm run dev              # 개발 서버 실행
# 브라우저: http://localhost:3000
# 코드 저장하면 자동으로 Turbopack이 재빌드함
```

자주 사용되는 명령어 조합:

```bash
# 린트 오류 자동 수정 후 개발 서버 실행
npm run lint:fix && npm run dev

# 포매팅 후 개발 서버 실행
npm run format && npm run dev
```

### 커밋 전

```bash
npm run check-all        # 모든 검사 통과 확인
npm run build            # 프로덕션 빌드 성공 확인
```

체크리스트:

- [ ] TypeScript 타입 에러 없음
- [ ] ESLint 경고/에러 없음
- [ ] Prettier 포매팅 일관성 (자동 처리)
- [ ] 반응형 디자인 테스트 (모바일/태블릿/데스크톱)
- [ ] 해당 Phase의 완료 기준 충족
- [ ] 의미 있는 커밋 메시지 작성

---

## 🎯 핵심 개발 패턴

### 1. TypeScript 타입 (필수)

- **금지**: `any` 타입 사용 금지
- **필수**: Props는 명시적 `interface` 또는 `type`으로 정의
- **필수**: Notion API 응답도 타입 정의

```typescript
// ✅ Good: Props 타입 정의
interface HeaderProps {
  title: string;
  logo?: string;  // optional
  className?: string;
}

export function Header({ title, logo, className }: HeaderProps) {
  return <header className={className}>{title}</header>;
}

// ✅ Good: 데이터 타입 정의
interface Post {
  id: string;
  slug: string;
  title: string;
  category: string;
  publishedDate: Date;
  content: string;
}
```

### 2. 컴포넌트 구조

- **함수형 컴포넌트만 사용** (클래스 컴포넌트 금지)
- **파일명**: PascalCase (Header.tsx, LoginForm.tsx)
- **export 방식**: `export function ComponentName()` 또는 `export const ComponentName = ()`

```typescript
// src/components/layout/header.tsx
interface HeaderProps {
  title: string;
}

export function Header({ title }: HeaderProps) {
  return <header>{title}</header>;
}
```

### 3. Styling (Tailwind CSS)

- **모든 스타일링은 Tailwind CSS 사용**
- 커스텀 CSS 파일 최소화 (정말 필요한 경우에만)
- Tailwind plugin이 자동으로 클래스 정렬 (prettier-plugin-tailwindcss)

```typescript
// ✅ Good: Tailwind 클래스만 사용
<div className="flex items-center justify-between p-4 bg-white dark:bg-slate-900">
  {/* content */}
</div>

// ❌ Bad: 커스텀 CSS 파일
// import styles from './styles.module.css';
```

### 4. Form Handling (React Hook Form + Zod)

- **필수**: React Hook Form + Zod 조합 사용
- **필수**: 클라이언트/서버 양쪽 검증

```typescript
import { useForm } from 'react-hook-form';
import { zodResolver } from '@hookform/resolvers/zod';
import { z } from 'zod';

const schema = z.object({
  email: z.string().email('유효한 이메일을 입력하세요'),
  password: z.string().min(8, '8자 이상이어야 합니다'),
});

type FormData = z.infer<typeof schema>;

export function LoginForm() {
  const form = useForm<FormData>({
    resolver: zodResolver(schema),
  });

  return <form onSubmit={form.handleSubmit(onSubmit)}>{/* ... */}</form>;
}
```

### 5. Server Actions & API 호출 (Phase 2 이후)

- **모든 Notion API 호출은 Server Actions으로 구현**
- **민감한 정보** (`NOTION_API_TOKEN`)는 서버 환경변수에서만 접근
- **클라이언트 컴포넌트**는 서버 함수만 호출

```typescript
// ✅ Good: Server Action (src/lib/notion.ts)
'use server'

import { Client } from '@notionhq/client'

export async function fetchPosts() {
  const notion = new Client({
    auth: process.env.NOTION_API_TOKEN,
  })
  // ... API 호출
}

// ❌ Bad: 클라이언트에서 API 키 노출
const token = process.env.NEXT_PUBLIC_NOTION_TOKEN
```

---

## 📂 중요 파일 및 모듈

| 파일                        | 목적                                | 수정 여부       |
| --------------------------- | ----------------------------------- | --------------- |
| `src/app/layout.tsx`        | 루트 레이아웃 (테마, 글로벌 스타일) | O (수정 가능)   |
| `src/lib/env.ts`            | 환경 변수 타입 정의 및 검증         | O (필요시 추가) |
| `src/lib/utils.ts`          | 재사용 가능한 유틸리티 함수         | O (필요시 추가) |
| `src/components/ui/*`       | shadcn/ui 자동 생성 컴포넌트        | X (수정 금지)   |
| `src/components/layout/*`   | 레이아웃 컴포넌트                   | O (수정 가능)   |
| `src/components/providers/` | Context Providers (테마 등)         | O (필요시 추가) |
| `docs/guides/`              | 개발 가이드 및 best practices       | O (참고 문서)   |

---

## 🚀 Phase-based Development

프로젝트는 5단계(Phase)로 구성됩니다. 각 Phase의 상세 체크리스트는 `docs/ROADMAP.md` 참고:

| Phase       | 내용                               | 예상 기간 |
| ----------- | ---------------------------------- | --------- |
| **Phase 1** | 프로젝트 기본 구조, UI 기초 (현재) | ✅ 완료   |
| **Phase 2** | Notion API 통합, 데이터 모델       | 진행 예정 |
| **Phase 3** | 글 목록, 상세 페이지 기능          | 진행 예정 |
| **Phase 4** | 필터링, 검색, SEO 최적화           | 진행 예정 |
| **Phase 5** | 성능 최적화, 배포 준비             | 진행 예정 |

---

## 🔗 문서 및 참고자료

### 공식 문서 (외부)

- [Next.js 15 Documentation](https://nextjs.org/docs)
- [React 19 Documentation](https://react.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Tailwind CSS Docs](https://tailwindcss.com/docs)
- [shadcn/ui Components](https://ui.shadcn.com/)
- [Notion API Reference](https://developers.notion.com/) (Phase 2 이후 필요)

### 프로젝트 문서 (docs/ 디렉토리)

- `docs/PRD.md` — 전체 프로젝트 요구사항 및 기능 명세
- `docs/ROADMAP.md` — Phase별 개발 계획 및 체크리스트
- `docs/guides/nextjs-15.md` — Next.js 15 심화 가이드
- `docs/guides/component-patterns.md` — 컴포넌트 패턴 및 best practices
- `docs/guides/styling-guide.md` — Tailwind CSS 스타일링 규칙
- `docs/guides/forms-react-hook-form.md` — React Hook Form + Zod 폼 처리
- `docs/guides/project-structure.md` — 프로젝트 구조 상세 설명

---

## 🆘 자주 마주치는 문제 해결

### TypeScript 타입 에러

```bash
npm run typecheck        # 타입 검사 실행
npm run typecheck        # 컴파일되지 않은 부분 찾기
```

**원인**: `any` 타입 사용, Props 타입 미정의  
**해결**: interface로 명시적 타입 정의

### ESLint 오류

```bash
npm run lint             # 린트 검사
npm run lint:fix         # 자동 수정 가능한 오류 고치기
```

**원인**: 코드 컨벤션 위반  
**해결**: eslint.config.mjs 규칙 확인

### Prettier 포매팅 오류

```bash
npm run format           # Prettier 포매팅 자동 적용
npm run format:check     # 현재 포매팅 검사만
```

**원인**: 코드 스타일 불일치  
**해결**: `npm run format` 실행

### 빌드 실패 (npm run build)

```bash
# 1단계: 캐시 제거
rm -r .next

# 2단계: 의존성 재설치
npm install

# 3단계: 상세 로그와 함께 빌드
npm run build -- --verbose
```

### 개발 서버 포트 충돌

```bash
# 포트 3001에서 실행
npm run dev -- -p 3001
```

### Tailwind CSS 스타일이 적용되지 않음

- 클래스명 철자 확인 (오타 확인)
- 동적 클래스명 사용 회피 (예: ``className={`text-${size}`}`` X)
- `npm run format` 실행하여 클래스 정렬
- 브라우저 캐시 삭제 (DevTools → Application → Clear Storage)

---

## 💡 개발 팁

### 빠른 개발 사이클

```bash
# 터미널 1: 개발 서버 실행 (자동 리로드)
npm run dev

# 터미널 2: 타입 체크 (백그라운드)
npm run typecheck

# 저장하면 자동으로 Turbopack이 컴파일 + 브라우저 새로고침
```

### 커밋 전 체크리스트 자동화

```bash
# 모든 검사를 한 번에 수행
npm run check-all

# 통과하면 커밋 진행
git add .
git commit -m "기능: 새로운 기능 추가"
```

### shadcn/ui 컴포넌트 추가

```bash
# Button 컴포넌트 추가 예시
npx shadcn@latest add button

# 추가되는 파일: src/components/ui/button.tsx
# tsconfig.json의 path alias가 자동으로 처리됨
```

---

**Last Updated**: 2026-05-19
