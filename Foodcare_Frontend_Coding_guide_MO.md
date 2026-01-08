# 🍽️ Foodcare-CLE Frontend Coding 가이드

![Version](https://img.shields.io/badge/version-1.0-blue.svg)
![Status](https://img.shields.io/badge/status-Complete-brightgreen.svg)
![License](https://img.shields.io/badge/license-MIT-green.svg)
![Pages](https://img.shields.io/badge/pages-12-orange.svg)

**작성일**: 2026년 1월 8일  
**대상 사이트**: [https://www.foodcare-cle.com/](https://www.foodcare-cle.com/)  
**문서 범위**: 12개 주요 페이지의 HTML, CSS, JavaScript 종합 가이드  
**대상 개발자**: Frontend 개발자, UI/UX 개발자, 웹 표준 준수 개발자

---

## 📑 Quick Navigation

- [소개](#소개)
- [사이트 개요](#사이트-개요)
- [개발 환경](#개발-환경-및-기술-스택)
- [공통 패턴](#공통-설계-패턴)
- [페이지 가이드](#페이지별-상세-가이드)
- [접근성](#접근성-및-웹-표준)
- [성능](#성능-최적화)
- [보안](#보안-고려사항)

---

## 소개

이 문서는 **푸드케어 클레(Foodcare-CLE)** 웹사이트의 프론트엔드 개발 가이드입니다.  
사이트의 주요 12개 페이지에 대한 상세한 HTML 구조, CSS 패턴, JavaScript 상호작용을 문서화합니다.

### 📌 문서의 목적

- ✅ 현재 구현된 기능 및 패턴 문서화
- ✅ 신규 기능 개발 시 참고할 수 있는 코드 예제 제공
- ✅ 접근성 및 웹 표준 준수 가이드 제시
- ✅ 팀 간 개발 표준화
- ✅ 코드 유지보수성 향상

---

## 사이트 개요

| 항목 | 설명 |
|------|------|
| **서비스** | 정기식 배송 기반 이유식 구독 서비스 |
| **고객** | 영아 부모 (생후 4개월~14개월) |
| **모델** | 정기구독 + 단품 판매 |
| **주요 기능** | 상품 구매, 배송 관리, 주문 조회, 쿠폰 관리 |

---

## 개발 환경 및 기술 스택

### Frontend 기술
```
- HTML5 (시맨틱 마크업)
- CSS3 (Flexbox, Grid, Media Queries)
- Vanilla JavaScript (ES6+)
- 프레임워크: 없음 (순수 JavaScript)
```

### 분석 & 추적
```
- Google Analytics 4 (GA4)
- Kakao Pixel
- BigInsight
- Channel IO (고객 상담)
```

### 환경
```
- 모바일 우선 반응형 디자인
- 터치 최적화
- 모바일 네비게이션 (드로어)
```

---

## 공통 설계 패턴

### 1️⃣ 페이지 구조

모든 페이지는 다음의 공통 구조를 따릅니다:
```html
<header>               <!-- 상단 네비게이션 & 검색 -->
  - 뒤로가기 버튼
  - 페이지 제목
  - 검색 아이콘
  - 장바구니 아이콘
</header>

<article>             <!-- 메인 콘텐츠 -->
  - 페이지별 특화 콘텐츠
</article>

<nav class="bottom-nav">  <!-- 하단 네비게이션 -->
  - 메뉴 / 홈 / 식단주문 / 스케줄관리 / 마이페이지
</nav>

<aside>               <!-- 드로어 메뉴 (모바일) -->
  - 사용자 정보
  - 메뉴 목록
  - 빠른 링크
</aside>

<footer>              <!-- 하단 정보 -->
  - 회사 정보 / 약관 / 고객센터 / 소셜 미디어
</footer>
```

### 2️⃣ 폼 처리 패턴
```html
<form>
  <div class="form-group">
    <label for="fieldId" class="form-label">필드명</label>
    <input type="text" 
           id="fieldId"
           name="fieldName"
           class="form-input"
           required />
    <div class="error-message" style="display: none;"></div>
  </div>
</form>
```

### 3️⃣ 상태 관리 패턴
```javascript
class PageManager {
  constructor() {
    this.state = {
      isLoading: false,
      error: null,
      data: null,
      filters: {},
      pagination: { page: 1, size: 10 }
    };
  }
  
  setState(updates) {
    this.state = { ...this.state, ...updates };
    this.render();
  }
  
  render() {
    // UI 업데이트
  }
}
```

### 4️⃣ API 호출 패턴
```javascript
async apiCall(endpoint, options = {}) {
  try {
    const response = await fetch(endpoint, {
      method: options.method || 'GET',
      headers: {
        'Content-Type': 'application/json',
        'X-CSRF-Token': this.getCsrfToken()
      },
      body: options.body ? JSON.stringify(options.body) : undefined
    });
    
    if (!response.ok) {
      throw new Error(`API Error: ${response.status}`);
    }
    
    return await response.json();
  } catch (error) {
    console.error('API 호출 실패:', error);
    throw error;
  }
}
```

### 5️⃣ 분석 추적 패턴
```javascript
// 페이지 뷰
dev_ext.gtag.push('page_view', {
  page_path: window.location.pathname,
  page_title: document.title
});

// 사용자 액션
dev_ext.gtag.push('user_action', {
  action_name: 'button_click',
  element: 'checkout_btn'
});

// 전환 추적
dev_ext.gtag.push('purchase', {
  transaction_id: orderId,
  currency: 'KRW',
  value: totalPrice
});
```

---

## 페이지별 상세 가이드

### 1. 홈페이지

**URL**: `/`  
**목적**: 사이트 소개 및 상품 발견  

**핵심 기능:**
- 상품 카테고리별 탐색
- 이미지 캐러셀 (배너)
- 실시간 상품 추천
- 검색 기능

**HTML 구조:**
```html
<!-- 이미지 캐러셀 -->
<section class="carousel-section">
  <div class="carousel-wrapper">
    <ul class="carousel-items">
      <li class="carousel-item">
        <img src="banner-1.jpg" alt="배너 1" />
      </li>
    </ul>
  </div>
</section>

<!-- 상품 그리드 -->
<section class="products-section">
  <h2>상품 카테고리</h2>
  <div class="product-grid">
    <article class="product-card">
      <img src="product.jpg" alt="상품명" />
      <h3>상품명</h3>
      <p class="price">가격</p>
    </article>
  </div>
</section>
```

**CSS 반응형:**
```css
/* 모바일 */
.product-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 12px;
}

/* 태블릿 */
@media (min-width: 768px) {
  .product-grid {
    grid-template-columns: repeat(3, 1fr);
  }
}

/* 데스크톱 */
@media (min-width: 1024px) {
  .product-grid {
    grid-template-columns: repeat(4, 1fr);
  }
}
```

**JavaScript:**
```javascript
class CarouselManager {
  init() {
    this.setupAutoPlay();
    this.setupSwipeGestures();
  }
  
  setupAutoPlay() {
    setInterval(() => this.nextSlide(), 5000);
  }
  
  setupSwipeGestures() {
    let startX = 0;
    this.carousel.addEventListener('touchstart', (e) => {
      startX = e.touches[0].clientX;
    });
    this.carousel.addEventListener('touchend', (e) => {
      const endX = e.changedTouches[0].clientX;
      if (startX - endX > 50) this.nextSlide();
      if (endX - startX > 50) this.prevSlide();
    });
  }
}
```

---

### 2. 상품 상세페이지

**URL**: `/shop/mealPlan/{CATEGORY}/{ID}`  
**복잡도**: ⭐⭐⭐⭐⭐ (가장 복잡한 페이지)

**주요 섹션:**
- 상품 이미지 & 기본 정보
- 배송 방식 선택
- 시작일 선택 (캘린더)
- 수량 선택
- 배송 빈도 선택
- 메뉴 선택
- 알러지 정보
- 고객 리뷰

**특화 기능:**
```javascript
class ProductDetailManager {
  constructor() {
    this.orderState = {
      shippingMethod: 'cle',
      startDate: null,
      quantity: 1,
      frequency: 'daily',
      selectedMenus: [],
      allergies: []
    };
  }
  
  // 실시간 가격 계산
  calculatePrice() {
    const basePrice = this.product.price;
    const frequency = this.orderState.frequency;
    const quantity = this.orderState.quantity;
    
    return basePrice * quantity * this.getFrequencyMultiplier(frequency);
  }
  
  // 메뉴 선택 처리
  selectMenu(date, menuId) {
    this.orderState.selectedMenus[date] = menuId;
    this.updateTotalPrice();
  }
}
```

---

### 3. 로그인 페이지

**URL**: `/member/login`
```html
<form class="login-form" method="post">
  <input type="email" name="email" placeholder="이메일" required />
  <input type="password" name="password" placeholder="비밀번호" required />
  
  <label>
    <input type="checkbox" name="rememberMe" />
    아이디 저장
  </label>
  
  <button type="submit">로그인</button>
  
  <!-- 소셜 로그인 -->
  <div class="social-login">
    <button type="button" class="kakao-login">카카오 로그인</button>
    <button type="button" class="google-login">Google 로그인</button>
  </div>
</form>
```

**보안:**
```javascript
// CSRF 토큰 포함
form.appendChild(
  Object.assign(document.createElement('input'), {
    type: 'hidden',
    name: 'token',
    value: csrfToken
  })
);

// 세션 토큰만 저장 (비밀번호는 절대 저장 금지)
sessionStorage.setItem('authToken', response.token);
```

---

### 4. 회원가입 페이지

**URL**: `/member/authentication`

**폼 검증 로직:**
```javascript
const validators = {
  email: (value) => /^[^\\s@]+@[^\\s@]+\\.[^\\s@]+$/.test(value),
  password: (value) => value.length >= 8 && /[A-Z]/.test(value),
  phone: (value) => /^\\d{10,11}$/.test(value.replace(/-/g, '')),
  name: (value) => value.length >= 2
};

function validateForm(formData) {
  const errors = {};
  
  Object.entries(validators).forEach(([field, validator]) => {
    if (!validator(formData[field])) {
      errors[field] = `${field} 형식이 잘못되었습니다.`;
    }
  });
  
  return errors;
}
```

---

### 5. 마이페이지

**URL**: `/mypage/`

**그리드 레이아웃:**
```css
.dashboard-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 12px;
}

.card {
  padding: 16px;
  background: white;
  border: 1px solid #e5e5e5;
  border-radius: 8px;
  text-align: center;
}

.card-value {
  font-size: 24px;
  font-weight: 700;
  color: #4caf50;
}

.card-label {
  font-size: 12px;
  color: #999;
  margin-top: 4px;
}
```

---

### 6. 네비게이션 메뉴

**형태**: 드로어 네비게이션 (모바일)
```html
<nav class="drawer-nav" aria-label="메인 메뉴">
  <div class="nav-header">
    <span class="user-name">우희선님</span>
    <button class="close-btn">×</button>
  </div>
  
  <ul class="nav-menu">
    <li><a href="/mypage/manageSchedule">스케줄관리</a></li>
    <li><a href="/mypage/coupon">쿠폰</a></li>
  </ul>
</nav>
```

**애니메이션:**
```css
.drawer-nav {
  position: fixed;
  left: -100%;
  top: 0;
  width: 100%;
  height: 100vh;
  transition: left 0.3s ease;
}

.drawer-nav.open {
  left: 0;
}
```

---

### 7. 장바구니

**URL**: `/shop/cart`

**구조:**
```html
<ul class="cart-items">
  <li class="cart-item" data-item-id="1405161">
    <label>
      <input type="checkbox" name="selected" />
    </label>
    
    <div class="item-content">
      <img src="product.jpg" alt="상품명" />
      <div>
        <h3>상품명</h3>
        <p class="options">옵션 정보</p>
        <span class="price">64,400원</span>
      </div>
    </div>
    
    <button class="delete-btn">×</button>
  </li>
</ul>
```

**가격 계산:**
```javascript
class CartManager {
  calculateTotal() {
    const selectedItems = this.getSelectedItems();
    
    const productTotal = selectedItems.reduce((sum, item) => {
      return sum + (item.price * item.quantity);
    }, 0);
    
    const discount = this.calculateDiscount(selectedItems);
    const shipping = this.calculateShipping(selectedItems);
    
    return {
      productTotal,
      discount,
      shipping,
      total: productTotal - discount + shipping
    };
  }
}
```

---

### 8. 주문서 작성

**URL**: `/shop/infoInput`  
**복잡도**: ⭐⭐⭐⭐⭐

**주요 섹션:**
1. 배송 정보
2. 할인/혜택
3. 결제 수단
4. 약관 동의

**동적 폼 필드:**
```javascript
class OrderForm {
  setupConditionalFields() {
    const shippingSelect = document.querySelector('[name="deliveryMethod"]');
    
    shippingSelect.addEventListener('change', (e) => {
      const vbankSection = document.querySelector('.vbank-section');
      
      if (e.target.value === 'vbank') {
        vbankSection.style.display = 'block';
      } else {
        vbankSection.style.display = 'none';
      }
    });
  }
}
```

---

### 9. 주문완료

**URL**: `/shop/paymentComplete`

**분석 이벤트 추적:**
```javascript
class PaymentCompletePage {
  trackPurchase() {
    const purchaseData = {
      transaction_id: orderId,
      currency: 'KRW',
      value: totalPrice,
      items: products
    };
    
    // Google Analytics
    dev_ext.gtag.push('purchase', purchaseData);
    
    // Kakao Pixel
    dev_ext.kakaoPixel.purchase({
      total_quantity: productCount,
      total_price: totalPrice
    });
  }
}
```

---

### 10. 주문조회

**URL**: `/mypage/orderHistory`

**필터 옵션:**
- 기간 선택 (1/3/6/12개월)
- 주문 상태 필터
- 선물주문 필터
```javascript
class OrderHistoryManager {
  filterState = {
    startDate: '2025.12.08',
    endDate: '2026.01.08',
    status: 'all',
    giftOrder: false,
    pageNo: 1,
    pageSize: 10
  };
  
  applyFilters() {
    this.resetPagination();
    this.loadOrders();
  }
}
```

---

### 11. 이벤트 상세

**URL**: `/event/eventDetail/{ID}`

**공유 기능:**
```javascript
class EventDetailPage {
  shareToKakao() {
    Kakao.Share.sendDefault({
      objectType: 'feed',
      content: {
        title: this.eventData.title,
        imageUrl: this.eventData.imageUrl,
        link: {
          mobileWebUrl: this.eventData.shareUrl
        }
      }
    });
  }
  
  copyUrlToClipboard() {
    navigator.clipboard.writeText(this.eventData.shareUrl);
  }
}
```

**카운트다운:**
```javascript
setupCountdown() {
  const deadline = new Date(this.eventData.endDate);
  
  const updateCountdown = () => {
    const now = new Date();
    const daysRemaining = Math.floor(
      (deadline - now) / (1000 * 60 * 60 * 24)
    );
    
    document.querySelector('.countdown').textContent = `D-${daysRemaining}`;
  };
  
  updateCountdown();
  setInterval(updateCountdown, 3600000); // 1시간마다
}
```

---

### 12. 쿠폰 페이지

**URL**: `/mypage/coupon`

**쿠폰 등록 폼:**
```html
<form class="coupon-registration-form">
  <input type="text" 
         placeholder="하이픈( - )없이 쿠폰번호를 입력해 주세요."
         maxlength="20" />
  <button type="submit">등록</button>
  
  <div class="error-message" role="alert" aria-live="polite">
    <!-- 에러 메시지 표시 -->
  </div>
</form>
```

**유효성 검증:**
```javascript
class CouponValidator {
  validateCouponNumber(couponNumber) {
    const cleaned = couponNumber.replace(/-/g, '');
    
    if (cleaned.length < 10) {
      return { valid: false, error: '쿠폰 번호가 너무 짧습니다.' };
    }
    
    return this.serverValidate(cleaned);
  }
}
```

---

## 접근성 및 웹 표준

### WCAG 2.1 준수 (레벨 AA)

✅ 모든 이미지에 대체 텍스트  
✅ 충분한 색상 대비 (4.5:1 이상)  
✅ 키보드 네비게이션 지원  
✅ ARIA 라벨 및 역할 지정

**ARIA 패턴:**
```html
<!-- 폼 검증 에러 -->
<div role="alert" aria-live="polite">
  잘못된 입력입니다.
</div>

<!-- 탭 컨트롤 -->
<div role="tablist">
  <button role="tab" aria-selected="true" aria-controls="panel1">
    탭 1
  </button>
  <div role="tabpanel" id="panel1">
    콘텐츠
  </div>
</div>

<!-- 모달 -->
<div role="dialog" aria-labelledby="dialog-title">
  <h2 id="dialog-title">모달 제목</h2>
</div>
```

**시맨틱 HTML:**
```html
<header>헤더</header>
<nav>네비게이션</nav>
<main>메인 콘텐츠</main>
<article>기사</article>
<section>섹션</section>
<footer>푸터</footer>
```

---

## 성능 최적화

### 1️⃣ 이미지 레이지 로딩
```javascript
const imageObserver = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      const img = entry.target;
      img.src = img.dataset.src;
      imageObserver.unobserve(img);
    }
  });
});

document.querySelectorAll('img[data-src]').forEach(img => {
  imageObserver.observe(img);
});
```

### 2️⃣ 디바운싱 & 쓰로틀링
```javascript
// 디바운싱
function debounce(func, wait) {
  let timeout;
  return function(...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func(...args), wait);
  };
}

// 쓰로틀링
function throttle(func, limit) {
  let inThrottle;
  return function(...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => inThrottle = false, limit);
    }
  };
}
```

### 3️⃣ DOM 배치 처리
```javascript
const fragment = document.createDocumentFragment();

items.forEach(item => {
  const element = document.createElement('div');
  element.textContent = item.name;
  fragment.appendChild(element);
});

container.appendChild(fragment); // 한 번의 리플로우만 발생
```

### 4️⃣ 캐싱 전략
```javascript
class CacheManager {
  set(key, value, ttl = 3600000) {
    const item = {
      value,
      expiry: Date.now() + ttl
    };
    localStorage.setItem(key, JSON.stringify(item));
  }
  
  get(key) {
    const item = JSON.parse(localStorage.getItem(key));
    
    if (!item) return null;
    if (Date.now() > item.expiry) {
      localStorage.removeItem(key);
      return null;
    }
    
    return item.value;
  }
}
```

---

## 보안 고려사항

### ✅ CSRF 방지
```html
<!-- CSRF 토큰 -->
<form method="post">
  <input type="hidden" name="token" value="{{csrfToken}}" />
</form>
```

### ✅ XSS 방지
```javascript
// 사용자 입력 이스케이프
function escapeHtml(text) {
  const div = document.createElement('div');
  div.textContent = text;
  return div.innerHTML;
}

// textContent 사용 (권장)
element.textContent = userInput;
```

### ✅ 민감한 정보 보호
```javascript
// 비밀번호는 절대 로그에 남기지 않음
console.log({ email: user.email }); // OK
console.log({ password: user.password }); // 금지

// 세션 토큰만 저장
sessionStorage.setItem('token', response.token);
```

### ✅ API 보안
```javascript
const apiCall = async (url, options = {}) => {
  const token = sessionStorage.getItem('token');
  
  const response = await fetch(url, {
    ...options,
    headers: {
      ...options.headers,
      'Authorization': `Bearer ${token}`,
      'X-CSRF-Token': getCsrfToken()
    }
  });
  
  if (response.status === 401) {
    window.location.href = '/member/login';
  }
  
  return response;
};
```

---

## 결론

이 문서를 따르면:

✅ **일관된 코드 품질** 유지  
✅ **접근성 표준** 준수  
✅ **성능 최적화** 달성  
✅ **보안** 강화  
✅ **유지보수성** 향상

### 📚 권장 사항

1. 분기별 코드 리뷰
2. 성능 모니터링 (Core Web Vitals)
3. 접근성 감사 (매년)
4. 보안 취약점 스캔
5. 사용자 피드백 수집

---

## 📋 페이지 체크리스트

- [x] 1. 홈페이지 (/)
- [x] 2. 상품 상세페이지 (/shop/mealPlan/{CATEGORY}/{ID})
- [x] 3. 로그인 페이지 (/member/login)
- [x] 4. 회원가입 페이지 (/member/authentication)
- [x] 5. 마이페이지 (/mypage/)
- [x] 6. 네비게이션 메뉴
- [x] 7. 장바구니 (/shop/cart)
- [x] 8. 주문서 작성 (/shop/infoInput)
- [x] 9. 주문완료 (/shop/paymentComplete)
- [x] 10. 주문조회 (/mypage/orderHistory)
- [x] 11. 이벤트 상세 (/event/eventDetail/{ID})
- [x] 12. 쿠폰 페이지 (/mypage/coupon)

---

## 📞 Contact & Support

- **문서 버전**: v1.0
- **마지막 업데이트**: 2026년 1월 8일
- **유지보수자**: 개발팀

---

## 📜 License

This documentation is provided as-is for the Foodcare-CLE development team.
```
MIT License - 2026 Foodcare-CLE
```

---

**Happy Coding! 🚀**
