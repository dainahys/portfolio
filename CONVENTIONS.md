# dainahys.com — 구조 & 디자인 아키텍처

## 디렉터리
```
index.html                     홈 (hero + Projects/Articles 카드, SPA)
assets/
  css/
    base.css                   ★ 디자인 토큰 단일 소스 (색·테마·폰트)
    project.css                프로젝트 케이스스터디 컴포넌트 시스템
  <slug>/                      프로젝트별 이미지 (soomgo, nextunicorn, promotion, leadgen, growth-infra ...)
articles/
  assets/article.css           아티클 컴포넌트 시스템 (@import base.css)
  _styleguide/index.html       아티클 컴포넌트 데모
  _template/index.html         새 아티클 스타터
  <topic>/<slug>/index.html    아티클 (예: braze/survey-modal/)
projects/
  _template/index.html         새 프로젝트 스타터
  <slug>/index.html            프로젝트 상세 (확장 시)
CNAME · robots.txt · sitemap.xml · og-image.png
```

## 디자인 토큰 (단일 소스 = `assets/css/base.css`)
- 모든 색은 `--accent*` 추상 토큰. 영역을 `data-theme`로 감싸면 액센트가 전환된다.
  - 미지정(기본) = **그 외 / promotion → 중립 잉크 #1F1F2B**
  - `data-theme="soomgo"` → **보라 #6A3AF2**
  - `data-theme="nextunicorn"` → **블루 #0064FF**
- 폰트: Display=Poppins · Body=Pretendard · Mono=DM Mono
- 컴포넌트 CSS는 전부 `var(--accent*)` 참조 → 토큰만 바꾸면 전체 반영.
- 인라인 SVG는 presentation 속성에서 `var()`가 안 먹으므로, 테마 고정 영역(아티클=중립)은 중립 hex 직접 사용.

## 테마 정책
- **메인 프로젝트**: 숨고=soomgo, 넥유=nextunicorn (브랜드색).
- **서브 프로젝트**(promotion·leadgen·growth-infra): 중립(기본).
- **아티클**: 회사 비노출 → 항상 **중립**(data-theme 미지정), 본문 회사명 텍스트도 지양.
- 홈: `#view-sg`·숨고 카드 = `data-theme="soomgo"`, `#view-nu`·넥유 카드 = `data-theme="nextunicorn"`, 나머지 중립.

## 새 아티클 추가
1. `articles/_template/` → `articles/<topic>/<slug>/index.html` 복사
2. `{{TITLE}} {{DESC}} {{SLUG_URL}} {{DATE}}` + JSON-LD·FAQ 채우기
3. 본문은 `_styleguide` 컴포넌트 스니펫 사용. `data-theme` 미지정(중립).
4. 홈 Articles 섹션(KO·EN)에 카드 1장 추가.

## 새 프로젝트 추가
1. `projects/_template/` → `projects/<slug>/index.html` 복사
2. `<body data-theme="...">` — 메인만 soomgo/nextunicorn, 서브는 속성 제거(중립)
3. `{{...}}` 채우고 이미지는 `assets/<slug>/`
4. 홈 Projects 카드에 `<a href="/projects/<slug>/">` 링크 추가
   (현재 홈은 인라인 SPA — 신규 프로젝트는 per-page 링크 방식 권장)

## 링크 순서
- 아티클/프로젝트 페이지: `project.css`(또는 `article.css`) 하나만 링크하면 `@import`로 base.css 자동 로드.
- 홈: `<link href="/assets/css/base.css">` 직접 링크.

## 유지보수 메모
- 대용량 이미지 다수(59MB) → WebP 변환·리사이즈 권장.
- 홈 index.html은 KO/EN 인라인 중복 → 장기적으로 데이터화 가능.
