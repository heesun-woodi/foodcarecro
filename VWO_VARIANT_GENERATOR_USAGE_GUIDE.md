# VWO Variant Generator Skill 사용 가이드

## 📦 스킬 개요

**vwo-variant-generator.skill**은 푸드케어 CLE 웹사이트의 A/B 테스트 실험을 위한 VWO 코드를 자동 생성하는 Claude Skill입니다.

### 주요 기능
- ✅ 실험설계서 PDF 분석
- ✅ **Playwright MCP로 실험 페이지 자동 분석** 🔍
- ✅ CSS Selector 자동 확보 및 검증
- ✅ VWO Code Editor용 JavaScript/CSS/HTML 코드 생성
- ✅ 깜빡임 방지 및 모바일 최적화
- ✅ 테스트 및 디버깅 지원
- ✅ 구현 히스토리 자동 기록

---

## 🗂️ 스킬 구조

```
vwo-variant-generator.skill
├── SKILL.md                                    # 메인 워크플로우 가이드
└── references/                                 # 참조 문서들
    ├── VWO_CODE_EDITOR_AI_GUIDE_ENHANCED.md   # VWO 코딩 베스트 프랙티스
    ├── Foodcare_Frontend_Coding_guide_MO.md   # 푸드케어 프론트엔드 가이드
    ├── foodcare_uiuxdesignguide.md            # UI/UX 디자인 가이드
    └── history.md                              # 구현 히스토리 (템플릿)
```

---

## 🚀 사용 방법

### Step 1: 스킬 업로드
1. Claude.ai에서 "Skills" 메뉴 열기
2. "Upload skill" 클릭
3. `vwo-variant-generator.skill` 파일 업로드

### Step 2: 실험 코드 생성

**실험설계서 PDF 제공:**
```
실험설계서 보고 VWO 코드 만들어줘 [PDF 첨부]
```

**Claude가 수행하는 작업:**
1. PDF 분석 → 실험 목표, 페이지, 변경사항 파악
2. **Playwright MCP로 페이지 분석** 🔍
   - 대상 URL 접속 및 DOM 구조 분석
   - CSS Selector 자동 확보 및 검증
   - 모바일 뷰포트 전환 후 레이아웃 확인
   - 변경 전 스크린샷 캡처
3. **참조 문서 확인** 📚
   - UI/UX 디자인 가이드 (색상, 폰트, 컴포넌트 규격)
   - 프론트엔드 코딩 가이드 (HTML/CSS/JS 패턴)
4. 부족한 정보 질문 (디자인 상세, 우선순위 등)
5. VWO 코드 생성 (HTML/CSS/JavaScript) - 디자인 시스템 적용
6. VWO 설정 가이드 및 테스트 체크리스트 제공

### Step 3: 테스트 및 수정

**문제 보고:**
```
테스트했는데 식단표 내용이 빈 칸이야
```

**Claude가 수행하는 작업:**
1. 진단 질문 (콘솔 로그, Live Preview 확인 등)
2. 문제 원인 파악
3. 수정된 코드 제공 + 설명

### Step 4: 성공 기록

**성공 알림:**
```
이제 잘 작동해!
```

**Claude가 수행하는 작업:**
1. `history.md`에 실험 내용, 코드, 히스토리 기록
2. 가이드 문서 업데이트 필요 여부 확인
3. 업데이트 제안 (사용자 승인 시 진행)

---

## 📋 워크플로우 상세

### 1단계: 실험설계서 분석
- PDF에서 추출:
  - 실험 목표 및 가설
  - 대상 페이지 URL 및 디바이스
  - Variant 수 및 변경사항
  - 성공 지표

### 2단계: Playwright MCP로 페이지 분석 🔍
**실험 대상 페이지를 실제로 분석하여 코딩에 필요한 정보를 확보합니다.**

```
🌐 페이지 분석 절차:
1. browser_navigate → 대상 URL 접속
2. browser_resize → 모바일 뷰포트 설정 (375x812)
3. browser_snapshot → DOM 구조 분석
4. browser_evaluate → CSS Selector 검증
5. browser_take_screenshot → 변경 전 화면 캡처
```

**수집하는 정보:**
- ✅ CSS Selectors (검증됨)
- ✅ DOM 구조 및 계층
- ✅ 기존 스타일 속성
- ✅ 동적 요소 여부
- ✅ 시각적 레이아웃

### 3단계: 정보 확인 및 보완
- Playwright 분석 결과 확인:
  - [x] CSS selectors (자동 확보)
  - [x] DOM 구조 (자동 확보)
  - [ ] 디바이스 타겟팅 (실험설계서)
  - [ ] 디자인 상세 (실험설계서)

### 4단계: 참조 문서 확인 📚
**푸드케어 디자인 시스템에 맞는 코드 생성을 위해 참조 문서를 확인합니다.**

```
📖 참조 문서 체크리스트:
□ foodcare_uiuxdesignguide.md
  - 색상: Primary #2B8B60, Text #333333
  - 버튼: 높이 48px, radius 4px
  - 폰트: Body 15px, Caption 12px
  - 여백: 페이지 16px, 섹션 24px

□ Foodcare_Frontend_Coding_guide_MO.md
  - HTML 구조 패턴
  - CSS 클래스 네이밍
  - JavaScript 패턴
  - 접근성 규칙
```

### 5단계: 코드 생성
**각 Variant별 생성:**
- HTML Tab: 새 요소 구조
- CSS Tab: 스타일링
- JavaScript Tab: 동적 로직 (IIFE 패턴)
- VWO Settings: Trigger, Hide elements

**자동 적용 사항:**
- ✅ 깜빡임 방지 (Hide elements)
- ✅ 모바일 디바이스 감지
- ✅ console.log 디버깅
- ✅ setInterval unique ID
- ✅ IIFE 캡슐화

### 4단계: 디버깅
**문제 진단 순서:**
1. Console 로그 확인
2. Live Preview 테스트
3. Selector 검증
4. Trigger 확인

### 5단계: 기록
**history.md 업데이트:**
- 실험 정보
- Variants 설명
- 구현 히스토리
- 학습 포인트

---

## 💡 활용 예시

### 예시 1: 식단표 가독성 개선
**입력:**
```
초기1 상품 상세페이지에서 "클레 식단표" 가독성 개선 실험 코드 만들어줘.
- V1: 모달에 식단표 전체 표시
- V2: 페이지에 직접 노출
- 모바일 전용
```

**출력:**
- V1 코드: 모달 최적화 (HTML/CSS/JS)
- V2 코드: 직접 노출 (HTML/CSS/JS)
- 디바이스 감지 로직
- VWO 설정 가이드
- 테스트 체크리스트

### 예시 2: 버튼 문구 변경
**입력:**
```
장바구니 페이지 "결제하기" 버튼을 "지금 구매하기"로 변경하는 실험
```

**출력:**
- JavaScript로 텍스트 변경 코드
- 깜빡임 방지 설정
- 테스트 가이드

---

## 🔧 VWO 설정 가이드

### 캠페인 생성
1. VWO 대시보드 → "Create" → "Test"
2. URL 입력: `https://www.foodcare-cle.com/shop/mealPlan/E/102`
3. Variant 추가: Control + V1 + V2

### 코드 적용
1. 각 Variant → "Edit Code" 클릭
2. **HTML Tab**: 생성된 HTML 붙여넣기
3. **CSS Tab**: 생성된 CSS 붙여넣기
4. **JavaScript Tab**: 생성된 JavaScript 붙여넣기
5. **Settings**:
   - Trigger: 지정된 트리거 선택
   - Hide elements: 셀렉터 입력

### 테스트
1. "Preview" 클릭 → 디바이스 선택
2. Debug Cookie 제거
3. 각 Variant 확인
4. 콘솔 로그 확인

---

## 📊 참조 문서 활용

### VWO_CODE_EDITOR_AI_GUIDE_ENHANCED.md
- VWO 코딩 베스트 프랙티스
- 깜빡임 방지 방법
- 트리거 선택 가이드
- 디버깅 전략
- **Playwright MCP 활용법** (섹션 1.5)
- **디자인 시스템 적용법** (섹션 1.6)

### Foodcare_Frontend_Coding_guide_MO.md
**코드 작성 시 반드시 참조!**
- 푸드케어 사이트 구조 (header → article → nav → aside → footer)
- 페이지별 HTML/CSS 패턴
- 공통 JavaScript 패턴 (PageManager, API 호출)
- 접근성 및 웹 표준 (ARIA, 시맨틱 HTML)

### foodcare_uiuxdesignguide.md
**디자인 값 적용 시 반드시 참조!**
| 항목 | 주요 값 |
|-----|--------|
| Primary Color | `#2B8B60` |
| Text Color | `#333333` / `#666666` / `#999999` |
| Border Color | `#E5E5E5` |
| Button Height | `48px` |
| Input Height | `44px` |
| Border Radius | Button `4px`, Card `8px` |
| Font Size | Body `15px`, Caption `12px` |
| Page Padding | `16px` |

### history.md
- 이전 실험 기록
- 자주 발생하는 문제
- 해결책 패턴
- 학습 포인트

---

## ⚠️ 주의사항

### 필수 체크사항
- [ ] "Hide element(s) until code runs" 항상 설정
- [ ] vwo_$() 래퍼 사용하지 않기
- [ ] setInterval 사용 시 unique ID
- [ ] 모바일 전용 실험은 디바이스 감지 추가
- [ ] console.log 디버깅 로그 포함

### 흔한 실수
- ❌ Hide elements 미설정 → 깜빡임 발생
- ❌ Selector 오타 → 요소 미발견
- ❌ PC에서도 실행 → 디바이스 체크 누락
- ❌ DOM 로딩 타이밍 → Retry 로직 필요

---

## 🎯 베스트 프랙티스

### 코드 품질
```javascript
// ✅ Good: IIFE + Early Return
(function() {
  'use strict';
  
  function isMobileDevice() { /* ... */ }
  if (!isMobileDevice()) return;
  
  console.log('[VWO] Mobile detected');
  // Main logic
})();

// ❌ Bad: 전역 변수, vwo_$() 사용
vwo_$(function() {
  var myVar = 'global'; // 전역 오염
  // ...
});
```

### Retry 패턴
```javascript
// ✅ Good: Retry + Timeout
var retryCount = 0;
var maxRetries = 10;
var intervalId = setInterval(function() {
  var element = document.querySelector('.dynamic');
  if (element) {
    // Apply changes
    clearInterval(intervalId);
  } else if (retryCount >= maxRetries) {
    clearInterval(intervalId);
    console.warn('[VWO] Element not found');
  }
  retryCount++;
}, 200);
```

---

## 📞 문의 및 지원

### 스킬 관련
- 스킬 동작 문제: Claude에게 직접 질문
- 워크플로우 개선 제안: 피드백 제공

### VWO 관련
- VWO 기술 지원: support@vwo.com
- 푸드케어 개발팀: 내부 문의

---

## 📝 버전 정보

- **스킬 버전**: 1.0
- **최초 작성**: 2026.01.11
- **최종 업데이트**: 2026.01.11
- **호환 사이트**: Foodcare-CLE (https://www.foodcare-cle.com/)

---

## 🎉 완료!

이제 실험설계서만 제공하면 Claude가 VWO 코드를 자동으로 생성합니다!

**시작하기:**
```
실험설계서 PDF를 Claude에게 업로드하고 
"VWO 코드 만들어줘"라고 요청하세요!
```
