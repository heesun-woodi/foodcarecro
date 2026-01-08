# VWO 코드 에디터 AI 코딩 에이전트 가이드 시스템 (Enhanced Version)

## 1.0 VWO 코드 에디터 핵심 원칙 및 구조

### 1.1 서론: VWO 코드 에디터의 역할과 목적

VWO 코드 에디터는 A/B 테스트, 기능 출시(Rollout), 개인화 캠페인에서 맞춤형 코드 변경을 구현해야 하는 사용자를 위한 고급 기능입니다. 이 가이드는 AI 에이전트가 에디터의 기능을 완벽하게 숙달하여 효율적이고, 깜빡임(flicker) 현상이 없으며, 안정적인 코드를 생성할 수 있도록 설계되었습니다. 비주얼 에디터의 범위를 넘어서는 복잡한 수정을 위해 코드 에디터를 사용하는 것은 전략적으로 중요합니다.

VWO에서 코드를 다루는 두 가지 기본 모드는 다음과 같습니다.

* **비주얼 에디터를 통한 코드 수정**: 비주얼 에디터에서 변경한 내용은 해당 JavaScript 코드 스니펫으로 생성되며, 이를 코드 에디터에서 추가로 수정할 수 있습니다.
* **코드 전용 모드(Code-only Mode)**: 고급 사용자가 맞춤형 베리에이션 코드를 처음부터 작성하기 위한 모드입니다. 이 방식으로 생성된 베리에이션은 비주얼 에디터에서 편집할 수 없습니다.

### 1.2 코드 에디터의 기본 구성 요소

VWO의 모든 코드는 명확한 계층 구조 내에서 구성됩니다. 이 구조를 이해하는 것은 유효한 코드를 생성하기 위한 첫 번째 단계입니다.

* **캠페인 코드 (Campaign Code)**: 테스트에 포함된 모든 페이지에서, 모든 베리에이션과 원본(control)에 걸쳐 실행될 JavaScript/CSS를 삽입합니다.
  * **사전 캠페인 JS (Pre-Campaign JS)**: 베리에이션 변경 사항이 적용되기 전에 실행됩니다.
  * **사후 캠페인 JS (Post-Campaign JS)**: 베리에이션 변경 사항이 적용된 후에 실행됩니다.
* **베리에이션 코드 (Variation Code)**: 단일 베리에이션에 특화된 코드가 추가되는 영역입니다. 여러 베리에이션이 존재할 수 있으며, 각 베리에이션은 자체적인 코딩 영역을 가집니다.
* **코드 블록 (Code Blocks)**: 베리에이션 내에서 코드 변경 사항을 배치하기 위한 개별적이고 체계적인 섹션입니다. 각기 다른 트리거와 구성을 가진 여러 변경 사항을 적용할 수 있습니다.

---

## 2.0 코드 블록(Code Blocks) 심층 분석

### 2.1 코드 블록의 구조와 실행 순서

코드 블록은 최신 VWO 에디터에서 코드 실행을 구성하고 제어하는 기본 메커니즘입니다. 코드 블록의 모듈식 특성은 복잡성을 관리하고 변경 사항이 정확한 시점에 적용되도록 보장하는 데 매우 중요합니다.

코드 블록 내에는 언어별로 다음과 같은 탭이 존재합니다.

1. **HTML**: 새로운 컴포넌트 구조를 추가하기 위해 사용됩니다.
2. **CSS**: 요소의 스타일을 지정하기 위해 사용됩니다.
3. **JavaScript**: 동적 변경 및 로직을 위해 사용됩니다.

단일 코드 블록 내의 코드는 항상 **HTML, 그다음 CSS, 마지막으로 JS** 순서로 실행됩니다. 이 순서는 변경할 수 없습니다.

**"비주얼 에디터 블록"**은 특별한 성격을 가집니다. 이는 비주얼 에디터에서 적용한 변경 사항으로부터 생성된 코드를 자동으로 포함하는 기본적이고 삭제할 수 없는 블록입니다. 이 블록의 트리거는 "캠페인 실행(Campaign executes)"으로 고정되어 변경할 수 없습니다.

### 2.2 코드 실행 트리거(Triggers)의 종류 및 활용

트리거를 전략적으로 선택하는 것은 매우 중요합니다. 올바른 트리거를 선택하면 대상 요소가 사용 가능할 때만 코드가 실행되도록 보장할 수 있으며, 이는 오류와 시각적 결함을 방지하는 핵심 요소입니다.

| 트리거 (Trigger) | 실행 시점 (Execution Point) | 주요 사용 사례 (Key Use Case) |
|-----------------|---------------------------|----------------------------|
| Campaign executes | 기본 트리거. 방문자의 브라우저에서 VWO 캠페인 코드가 실행될 때 실행됩니다. | 페이지 로드 시 즉시 사용 가능한 요소에 변경 사항을 적용하는 경우,<br>예: 홈페이지 히어로 배너 변경 |
| Element loaded | 특정 요소(셀렉터로 식별)가 페이지에 로드될 때 실행됩니다. | 사용자의 특정 행동(예: 버튼 클릭) 후에만 나타나는 모달 또는 팝업 내의 콘텐츠를 수정하는 경우 |
| DOM ready | 전체 문서 객체 모델(DOM)이 브라우저에 완전히 로드되었을 때 실행됩니다. | 요소가 느리게 로드될 수 있는 콘텐츠가 많은 페이지에서,<br>모든 요소가 조작 가능해진 후에 변경 사항을 적용하는 경우 |
| Run after another code block | 지정된 이전 코드 블록의 실행이 완료된 직후에 실행됩니다. | 한 가지 변경이 성공적으로 완료된 후에 다른 변경이 시작되어야 하는 종속적인 작업을 순서대로 실행하는 경우 |
| Campaign exit | 방문자가 캠페인에 포함되지 않은 페이지로 이동할 때 실행됩니다. | 단일 페이지 애플리케이션(SPA)에서 사용자가 테스트 페이지를 벗어날 때 변경 사항을 되돌리거나 필터를 재설정하는 경우 |

**NOTE**: "Element loaded" 트리거를 사용할 때, VWO는 기본적으로 변경 사항을 한 번만 적용합니다. 만약 요소가 로드될 때마다 변경 사항을 다시 적용해야 한다면, 반드시 "Keep applying changes" 옵션을 활성화해야 합니다.

---

## 3.0 AI 에이전트를 위한 핵심 코딩 모범 사례 및 규칙

### 3.1 깜빡임 현상 (Flicker) 방지

깜빡임 현상(원본 콘텐츠가 순간적으로 보이는 현상)은 맞춤형 코드로 변경 사항을 적용할 때 가장 흔하고 방해가 되는 문제입니다. 이를 방지하기 위해 다음 규칙은 반드시 준수해야 합니다.

깜빡임 현상을 방지하기 위한 기본 규칙은 **'숨기기-적용-표시' 순서**를 구현하는 것입니다. VWO는 맞춤형 코드가 어떤 요소를 수정할지 예측할 수 없으므로, 스크립트의 로직은 반드시 다음과 같아야 합니다:

1. 대상 요소를 즉시 숨깁니다.
2. 모든 수정을 적용합니다.
3. 수정된 요소를 표시합니다.

이 '숨기기-적용-표시' 프로세스를 자동화하고 수동 구현을 피하기 위해, VWO는 각 코드 블록 내에 **'Hide element(s) until code runs'** 옵션을 제공합니다. 이것이 맞춤형 코드를 사용할 때 깜빡임 현상을 방지하는 기본적이고 필수적인 방법입니다. AI는 이 필드에 숨길 요소의 CSS 셀렉터 경로를 지정해야 합니다. VWO가 숨길 셀렉터를 제안할 수도 있습니다.

**CRITICAL**: 비주얼 에디터를 사용하면 VWO가 자동으로 수정할 요소를 인식하여 깜빡임을 방지합니다. 그러나 맞춤형 코드를 사용할 때는 VWO가 어떤 요소를 수정할지 알 수 없기 때문에, 반드시 "Hide element(s) until code runs" 옵션을 활용해야 합니다.

**ATTENTION**: 전역 캠페인 설정에서 "Make modified elements visible when" 옵션이 "Always (elements are not hidden)"로 설정된 경우, "Hide element(s) until code runs" 옵션은 아무런 효과가 없습니다.

### 3.2 비동기 및 동적 콘텐츠 처리

초기 DOM 로드 이벤트 이후에 동적으로 로드되는 요소에 코드를 적용하는 것은 어려운 과제입니다. VWO의 이벤트 기반 트리거("Element loaded" 등)가 복잡한 다단계 비동기 작업에 불충분할 경우, 코드 블록 내에서 `setInterval()` 메서드를 사용하여 원하는 요소가 사용 가능한지 주기적으로 확인하는 폴링 메커니즘을 사용해야 합니다.

**규칙**: 동일한 URL에서 실행되는 여러 캠페인에서 `setInterval()`을 사용하는 경우, 각각 고유한 인터벌 ID 또는 변수 이름을 사용해야 합니다. 이를 지키지 않으면 한 캠페인의 `clearInterval()` 함수가 다른 캠페인의 인터벌을 방해할 수 있습니다. 각 캠페인에 고유한 인터벌 ID가 추가되도록 보장하려면, VWO 에디터의 "Add setInterval function" 헬퍼 기능을 사용하십시오. 이 기능은 충돌을 방지하기 위한 고유 ID를 포함한 코드를 생성합니다.

#### setInterval 사용 시나리오 및 모범 사례

**시나리오 1: DOM Ready 이후 로드되는 요소**

일부 요소는 DOM Ready 이벤트 이후에 비동기적으로 로드됩니다. 예를 들어:
- AJAX 호출로 로드되는 콘텐츠
- 지연 로드(lazy loading)되는 이미지나 컴포넌트
- 사용자 인터랙션 후 표시되는 동적 요소

이러한 경우, `vwo_$()` 래퍼나 DOM Ready 트리거만으로는 충분하지 않으며, `setInterval()`을 사용하여 요소의 존재를 주기적으로 확인해야 합니다.

**예시 코드 패턴:**

```javascript
// VWO 에디터의 "Add setInterval function" 사용 권장
var vwoIntervalID_123 = setInterval(function() {
  var targetElement = document.querySelector('.dynamic-element');
  
  if (targetElement) {
    // 요소를 찾았으면 변경 적용
    targetElement.style.backgroundColor = 'blue';
    
    // 인터벌 종료
    clearInterval(vwoIntervalID_123);
    
    console.log('[VWO] Changes applied to dynamic element');
  }
}, 100); // 100ms마다 확인

// 안전장치: 10초 후 자동 종료
setTimeout(function() {
  clearInterval(vwoIntervalID_123);
  console.warn('[VWO] Interval timeout - element not found within 10s');
}, 10000);
```

**주의사항:**
- 반드시 요소를 찾은 후 `clearInterval()`을 호출하여 불필요한 CPU 사용을 방지합니다.
- 타임아웃 메커니즘을 추가하여 무한 루프를 방지합니다.
- 고유한 변수명(예: `vwoIntervalID_123`)을 사용하여 다른 캠페인과의 충돌을 방지합니다.

### 3.3 CSS 및 jQuery 사용 규칙

VWO 환경 내에서 CSS 및 jQuery를 사용할 때는 다음의 엄격한 규칙을 따라야 합니다.

* **!important 속성**: 표준 jQuery `.css()` 함수는 `!important` 속성을 지원하지 않습니다. AI는 VWO 전용 함수인 `.vwoCss()`를 사용할 때만 이 속성을 사용해야 합니다.

**올바른 사용 예:**
```javascript
vwo_$('.selector').vwoCss('property', 'value !important');
```

**잘못된 사용 예 (작동하지 않음):**
```javascript
vwo_$('.selector').css('property', 'value !important');
```

* **vwo_$() 래퍼**: 규칙: 맞춤형 코드를 `vwo_$()` 함수로 감싸지 마십시오. 이 관행은 더 이상 사용되지 않으며, 코드를 DOM Ready 시점에 지연 실행시켜 페이지 초기 로드 시 보이는 요소("above the fold")에 깜빡임 현상을 유발하는 주요 원인이 됩니다.
* **insertAfter() / insertBefore()**: 이 함수들은 `vwo_$` 객체에서는 사용할 수 없습니다. AI는 사용하려는 `$/jQuery` 객체에서 이 함수들이 사용 가능한지 확인해야 합니다.

💡 **Expert Insight: vwo_$()가 깜빡임을 유발하는 이유**

`vwo_$()` 래퍼는 전체 DOM이 파싱된 후에만 코드가 실행되도록 보장하는 레거시 함수입니다. 이는 요소의 존재를 보장하지만, 페이지 상단("above the fold") 콘텐츠에 대한 경쟁 상태(race condition)를 만듭니다. 브라우저는 DOM Ready 이벤트가 발생하고 스크립트가 실행되기 전에 이미 원본 콘텐츠를 렌더링할 수 있으며, 이로 인해 변경 사항이 늦게 적용되면서 눈에 보이는 "깜빡임"이 발생합니다. "Element loaded"와 같은 최신 트리거나 "Hide element(s)" 기능을 사용하는 현대적인 모범 사례는 더 빠르고 정확하게 작동하여 이 경쟁에서 이기도록 설계되었습니다.

### 3.4 디바이스별 타겟팅

특정 디바이스(모바일, 태블릿, 데스크톱)에서만 코드를 실행하려면, JavaScript의 디바이스 감지 로직을 코드 블록 최상단에 추가해야 합니다.

**모바일 전용 실행 예시:**

```javascript
(function() {
  'use strict';
  
  // 디바이스 감지 함수
  function isMobileDevice() {
    var isMobileUA = /Android|webOS|iPhone|iPad|iPod|BlackBerry|IEMobile|Opera Mini/i.test(navigator.userAgent);
    var isMobileWidth = window.innerWidth <= 768;
    return isMobileUA || isMobileWidth;
  }
  
  // 모바일이 아니면 즉시 종료 (Early Return)
  if (!isMobileDevice()) {
    console.log('[VWO] PC device - script terminated');
    return;
  }
  
  console.log('[VWO] Mobile device - executing changes');
  
  // 모바일 전용 코드 실행
  // ... 나머지 코드
})();
```

**주요 포인트:**
- IIFE(즉시실행함수) 내에서 `return`을 사용하면 전체 스크립트가 종료됩니다.
- User Agent와 화면 너비를 모두 체크하여 정확도를 높입니다.
- VWO UI의 디바이스 타겟팅과 중복 설정하지 않습니다 (코드 레벨에서 제어).

---

## 4.0 주요 코드 작업 및 유틸리티

### 4.1 표준 에디터 작업 코드 스니펫

VWO 비주얼 에디터에서 수행하는 모든 작업에 대해 해당하는 JavaScript 코드 스니펫이 생성됩니다. 이 코드는 코드 에디터에서 확인하고 수정할 수 있습니다.

코드가 생성되는 표준 VWO 에디터 작업 목록은 다음과 같습니다.

* Move (이동)
* Show/Hide (Visibility) (표시/숨기기)
* Remove (제거)
* Change Image (이미지 변경)
* Change URLs (URL 변경)
* Change CSS (Styling) (CSS 변경)
* Content (Edit / Edit HTML) (콘텐츠 편집)
* Change Background Image (배경 이미지 변경)
* Paste (Before An Element) (요소 앞에 붙여넣기)
* Paste (After An Element) (요소 뒤에 붙여넣기)
* Rearrange (재배치)
* Custom Code to Remove An Element from Your DOM (DOM에서 요소 제거를 위한 맞춤 코드)

### 4.2 새로운 요소 추가

Library > VWO Gallery를 사용하여 코드를 작성하지 않고도 페이지에 새로운 요소를 추가할 수 있지만, 프로그래밍 방식으로 추가하는 것도 가능합니다. VWO Gallery를 통해 추가할 수 있는 요소에는 CTA나 이미지와 같은 기본(Basic) 요소, 제목이나 단락과 같은 타이포그래피(Typography) 요소, 그리고 입력 필드와 같은 폼(Form) 요소가 포함됩니다.

코드를 통해 새로운 요소를 추가하는 방법은 다음과 같습니다.

* **HTML 추가**: Edit Code > HTML 섹션을 사용하여 맞춤형 HTML을 추가할 수 있습니다. 새로운 콘텐츠는 기본적으로 페이지 하단에 추가되므로, 이후에 이동하거나 재배치해야 합니다.
* **비디오 추가**: UI에서 "Video" 요소를 제공하지만, YouTube나 Vimeo URL은 지원하지 않습니다. AI는 이러한 서비스의 비디오를 삽입하기 위해 VWO Gallery의 Widgets 섹션에 있는 YouTube 및 Vimeo 위젯을 사용해야 합니다.

### 4.3 고급 유틸리티 및 설정

이 섹션에서는 특정 시나리오를 처리하고 코드 품질을 향상시키기 위한 코드 에디터 내의 전문적인 도구와 설정을 다룹니다.

**Single-Page Application (SPA) 변경 사항 되돌리기 (Reverting Changes)**

SPA에서는 경로 변경 시 페이지가 완전히 새로고침되지 않는 지속적인 DOM에 변경 사항이 적용되기 때문에, 사용자가 캠페인 페이지를 벗어난 후에도 변경 사항이 유지될 수 있습니다. 이 문제를 해결하기 위해 "Add code to revert changes" 링크를 사용하거나, "Campaign exit" 트리거가 적용된 별도의 코드 블록을 사용할 수 있습니다.

**외부 라이브러리 포함 (Including External Libraries)**

Options > Include External JavaScript/Stylesheet 메뉴를 사용하여 외부 JavaScript 또는 CSS 파일을 포함할 수 있습니다.

**코드 품질 도구 (Code Quality Tools)**

다음 에디터 설정의 목적은 코드 품질을 향상시키는 것입니다.

* **Enable Code Linting**: 프로그래밍 및 스타일 오류를 자동으로 검사합니다.
* **Show Code Suggestions**: JS 코드 요소에 대한 자동 완성을 제공합니다.
* **Minify JavaScript/CSS code**: 불필요한 문자를 제거하여 파일 크기를 줄이고 로드 시간을 개선합니다.
* **Wrap Code**: 긴 JS 코드 라인의 가독성을 향상시킵니다.

---

## 5.0 디버깅 및 품질 보증 (QA) 모범 사례

### 5.1 console.log를 활용한 디버깅

변경 사항이 간헐적으로 적용되거나 예상대로 작동하지 않는 경우, 코드 실행 여부를 확인하는 것이 첫 번째 디버깅 단계입니다.

**디버깅 전략:**

1. **코드 실행 확인**: 맞춤형 코드에 `console.log()` 문을 추가하여 VWO가 코드를 실행하고 있는지 확인합니다.

```javascript
console.log('[VWO Debug] Script started');

var targetElement = document.querySelector('.my-element');

if (targetElement) {
  console.log('[VWO Debug] Element found:', targetElement);
  targetElement.style.color = 'red';
  console.log('[VWO Debug] Style applied successfully');
} else {
  console.warn('[VWO Debug] Element not found');
}

console.log('[VWO Debug] Script completed');
```

2. **로그 분석**:
   - **로그가 정상적으로 표시되는 경우**: VWO는 코드를 성공적으로 실행하고 있으므로, 맞춤형 코드 자체에 문제가 있을 가능성이 높습니다.
   - **로그가 전혀 표시되지 않는 경우**: VWO 설정(URL 타겟팅, 세그먼트, 트리거 등) 또는 캠페인 활성화 상태를 확인해야 합니다. 문제가 지속되면 support@vwo.com으로 문의하십시오.

3. **일반적인 문제 진단**:
   - 요소를 찾지 못하는 경우 → 셀렉터가 정확한지, 요소가 로드되는 타이밍이 맞는지 확인
   - DOM Ready 콜백 내에서 실행 중인 경우 → 요소가 DOM Ready 이후에 로드되는지 확인하고, 필요시 `setInterval()` 사용

**BEST PRACTICE**: 프로덕션 배포 전 디버깅 로그는 제거하거나 조건부로 활성화하여 성능에 영향을 주지 않도록 합니다.

### 5.2 Live Preview를 통한 검증

캠페인을 실제 사용자에게 활성화하기 전에, **Live Preview** 기능을 사용하여 변경 사항이 올바르게 적용되는지 반드시 검증해야 합니다.

**Live Preview 사용 절차:**

1. **캠페인 생성 완료 후** → 'Preview' 버튼 클릭
2. **디바이스 선택**: 데스크톱, 모바일, 태블릿 중 선택
3. **베리에이션 선택**: Control vs. Variation 간 전환하며 비교
4. **검증 항목**:
   - 변경 사항이 의도한 대로 표시되는가?
   - 깜빡임 현상이 발생하지 않는가?
   - 레이아웃이 깨지지 않았는가?
   - 다른 페이지 요소에 의도하지 않은 영향이 없는가?

**CRITICAL: Debug Cookie 제거**

Live Preview 테스트 전에 반드시 VWO Debug Cookie를 제거해야 합니다. Debug Cookie가 남아 있으면 캠페인 동작이 실제 환경과 다르게 나타날 수 있습니다.

**Debug Cookie 제거 방법:**
- VWO Chrome Extension 사용 시: Extension 아이콘 클릭 → "Clear VWO data" 선택
- 수동 제거: 브라우저 개발자 도구 → Application/Storage → Cookies → `_vis_opt_` 로 시작하는 쿠키 삭제

**RECOMMENDATION**: 여러 브라우저와 디바이스에서 Live Preview를 테스트하여 크로스 브라우저 호환성을 확인하십시오.

### 5.3 Segmentation 조건 검증

캠페인에서 세그먼트(타겟팅 조건)를 사용하는 경우, 세그먼트 조건이 올바르게 작동하는지 확인하는 것이 중요합니다.

**세그먼트 테스트 방법:**

1. **Preview 설정 변경**:
   - Live Preview 화면에서 설정(톱니바퀴) 아이콘 클릭
   - **"Ignore campaign targeting conditions in previews"** 옵션 **비활성화**

2. **조건 충족 테스트**:
   - 비활성화 후, Preview는 실제 세그먼트 조건을 평가합니다.
   - 조건이 충족되지 않으면 → 변경 사항이 표시되지 않고 경고 메시지가 나타납니다.
   - 조건이 충족되면 → 변경 사항이 정상적으로 표시됩니다.

**경고 메시지 예시:**
```
⚠️ Changes are not visible because segmentation conditions are not met.
Current visitor does not match the targeting rules.
```

**활용 사례:**
이 기능은 다음과 같은 경우 문제의 원인을 구분하는 데 유용합니다:
- 변경 사항이 표시되지 않는 이유가 **세그먼트 조건** 때문인지
- 아니면 **코드 자체**에 문제가 있는지

**디버깅 시나리오:**
```
문제: Live Preview에서 변경 사항이 보이지 않음

→ Step 1: "Ignore targeting conditions" 활성화
   - 변경 사항 보임 → 세그먼트 조건 문제
   - 변경 사항 안 보임 → 코드 문제

→ Step 2: Console 로그 확인
   - 로그 있음 → 코드 로직 문제
   - 로그 없음 → VWO 설정 문제 (URL 타겟팅, 트리거 등)
```

**BEST PRACTICE**: 복잡한 세그먼트 조건(다중 조건, 커스텀 이벤트 기반 등)을 사용하는 경우, 각 조건을 개별적으로 테스트하여 어느 조건이 문제를 일으키는지 파악하십시오.

### 5.4 크로스 브라우저 및 크로스 디바이스 테스트

**필수 테스트 매트릭스:**

| 디바이스 | 브라우저 | 우선순위 |
|---------|---------|---------|
| Desktop | Chrome | 🔴 High |
| Desktop | Safari | 🔴 High |
| Desktop | Firefox | 🟡 Medium |
| Desktop | Edge | 🟡 Medium |
| Mobile (iOS) | Safari | 🔴 High |
| Mobile (Android) | Chrome | 🔴 High |
| Mobile (Android) | Samsung Internet | 🟡 Medium |
| Tablet (iPad) | Safari | 🟡 Medium |

**테스트 체크리스트:**
- [ ] 깜빡임 현상 없음
- [ ] 레이아웃 정상 (responsive)
- [ ] 이미지/비디오 로딩 정상
- [ ] 폰트 렌더링 정상
- [ ] 인터랙션(클릭, 스크롤 등) 정상 작동
- [ ] 페이지 로딩 속도 영향 최소화

---

## 6.0 성능 최적화 지침

### 6.1 코드 효율성

**최적화 원칙:**

1. **DOM 조작 최소화**: 가능한 한 한 번의 작업으로 여러 변경 사항을 적용합니다.

```javascript
// ❌ 비효율적
element.style.color = 'red';
element.style.fontSize = '16px';
element.style.fontWeight = 'bold';

// ✅ 효율적
element.style.cssText = 'color: red; font-size: 16px; font-weight: bold;';
```

2. **셀렉터 최적화**: ID 셀렉터나 클래스 셀렉터를 우선 사용합니다.

```javascript
// ❌ 느림
document.querySelector('div > p > span.highlight');

// ✅ 빠름
document.getElementById('highlight-span');
// 또는
document.querySelector('.highlight');
```

3. **불필요한 setInterval 제거**: 요소를 찾은 후 반드시 `clearInterval()`을 호출합니다.

4. **코드 압축**: VWO의 "Minify JavaScript/CSS code" 옵션을 활성화합니다.

### 6.2 로딩 성능 고려사항

**체크 포인트:**
- 외부 라이브러리 최소화
- 큰 이미지 사용 시 최적화 (WebP, 압축)
- 복잡한 애니메이션 지양
- Heavy JavaScript 로직 최소화

**성능 측정:**
```javascript
console.time('[VWO] Execution time');
// 코드 실행
console.timeEnd('[VWO] Execution time');
```

---

## 7.0 체크리스트 및 빠른 참조

### 7.1 코드 작성 전 체크리스트

- [ ] 적절한 트리거 선택 (Campaign executes vs. Element loaded 등)
- [ ] "Hide element(s) until code runs" 셀렉터 지정
- [ ] setInterval 사용 시 고유한 변수명 사용
- [ ] vwo_$() 래퍼 사용하지 않음
- [ ] 디바이스별 타겟팅 필요 시 감지 로직 추가
- [ ] console.log 디버깅 문구 추가

### 7.2 배포 전 체크리스트

- [ ] Live Preview로 모든 베리에이션 확인
- [ ] Debug Cookie 제거 후 재테스트
- [ ] 세그먼트 조건 검증 ("Ignore targeting" 비활성화)
- [ ] 크로스 브라우저 테스트 (Chrome, Safari 필수)
- [ ] 모바일 디바이스 실제 테스트
- [ ] 깜빡임 현상 최종 확인
- [ ] 페이지 로딩 속도 영향 확인
- [ ] 프로덕션 배포 전 디버깅 로그 제거

### 7.3 문제 발생 시 진단 순서

1. **Console 확인** → 로그 있는가?
   - Yes → 코드 로직 문제
   - No → Step 2

2. **Live Preview 확인** → 변경 사항 보이는가?
   - Yes → 세그먼트 또는 URL 타겟팅 문제
   - No → Step 3

3. **"Ignore targeting" 활성화** → 변경 사항 보이는가?
   - Yes → 세그먼트 조건 재검토
   - No → Step 4

4. **트리거 확인** → 올바른 트리거 사용 중인가?
   - 동적 요소 → "Element loaded" 또는 setInterval
   - 즉시 로드 → "Campaign executes"

5. **셀렉터 확인** → 요소가 실제로 존재하는가?
   - Console에서 `document.querySelector('.selector')` 테스트

6. **VWO 지원팀 문의** → support@vwo.com

---

## 8.0 자주 발생하는 실수 및 해결책

### 8.1 일반적인 실수

| 실수 | 문제 | 해결책 |
|------|------|-------|
| vwo_$() 사용 | Above-the-fold 깜빡임 | 제거하고 "Element loaded" 트리거 사용 |
| Hide elements 미설정 | 원본 콘텐츠 노출 | "Hide element(s) until code runs" 설정 |
| setInterval ID 중복 | 다른 캠페인과 충돌 | 고유한 변수명 사용 |
| !important with .css() | 스타일 미적용 | .vwoCss() 함수로 변경 |
| DOM Ready 의존 | 동적 요소 미감지 | setInterval 폴링 사용 |
| 셀렉터 오타 | 요소 미발견 | Console에서 검증 |

### 8.2 고급 디버깅 기법

**MutationObserver 활용:**

동적으로 추가되는 요소를 감지하는 더 효율적인 방법:

```javascript
var observer = new MutationObserver(function(mutations) {
  mutations.forEach(function(mutation) {
    mutation.addedNodes.forEach(function(node) {
      if (node.classList && node.classList.contains('target-class')) {
        console.log('[VWO] Target element added');
        // 변경 적용
        node.style.backgroundColor = 'blue';
        observer.disconnect(); // 완료 후 종료
      }
    });
  });
});

observer.observe(document.body, {
  childList: true,
  subtree: true
});
```

---

## 9.0 결론

이 가이드는 VWO 코드 에디터를 사용하여 고품질의 A/B 테스트 베리에이션을 생성하기 위한 종합적인 지침을 제공합니다. AI 에이전트는 다음 핵심 원칙을 항상 기억해야 합니다:

1. **깜빡임 방지가 최우선**: "Hide element(s) until code runs" 항상 설정
2. **vwo_$() 사용 금지**: 레거시 함수이며 깜빡임 유발
3. **적절한 트리거 선택**: 요소의 로딩 시점에 맞춰 선택
4. **충분한 테스트**: Live Preview + 실제 디바이스 테스트 필수
5. **디버깅 우선**: console.log로 문제 원인 파악

이 가이드를 준수함으로써, AI 에이전트는 안정적이고 효율적이며 사용자 경험을 저해하지 않는 VWO 캠페인 코드를 생성할 수 있습니다.

---

**버전**: Enhanced v1.1  
**최종 업데이트**: 2026.01.08  
**작성자**: AI Coding Agent Guide Team
