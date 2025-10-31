<template>
  <!-- 전체 래퍼 -->
  <div class="promo-page">
    <!-- 떠다니는 풍선 레이어 -->
    <div class="floating-balloons" aria-hidden="true">
  <div class="balloon b1"></div>
  <div class="balloon b2"></div>
  <div class="balloon b3"></div>
  <div class="balloon b4"></div>
  <div class="balloon b5"></div>
  <div class="balloon b6"></div>
</div>


    <!-- 실제 콘텐츠 -->
    <div class="promo-banner">
      <img src="/public/images/promotion/welcome.png" alt="welcome event" />
    </div>

    <div class="promo-inner">
      <div class="promo-benefit">
        <div class="promo-title">
          <h1>마타주</h1>
          <h2>신규 회원 전용 혜택</h2>
          <!-- <p>신규회원 가입 시 누릴 수 있는 다양한 혜택을 확인해보세요!</p> -->
        </div>

        <!-- 상단 4개 베네핏 카드 -->
        <div class="benefit-list">
          <!-- benefit 1 -->
          <div class="benefit1">
            <div class="fit-title">
              <h1>Benefit</h1>
              <p>
                <svg class="emoji" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M128,24A104,104,0,1,0,232,128,104.2,104.2,0,0,0,128,24Zm12,152a8,8,0,0,1-16,0V98.9l-11.6,7.8a8,8,0,0,1-8.8-13.4l24-16a8.3,8.3,0,0,1,8.2-.4A8,8,0,0,1,140,84Z"
                    fill="currentColor"
                  />
                </svg>
              </p>
            </div>

            <div class="benefit-text">
              <span class="fit-text1">신규 멤버십 쿠폰</span> <br />
              <span class="fit-text2">5,000원 할인쿠폰 지급</span>
              <span class="fit-text2">* 유효기간 : 발급일로부터 10일</span>
            </div>

            <div class="benefit1-img">
              <img src="/public/images/promotion/be1.png" alt="benefit img 1" />
            </div>
          </div>

          <!-- benefit 2 -->
          <div class="benefit2">
            <div class="fit-title">
              <h1>Benefit</h1>
              <p>
                <svg class="emoji" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M128,24A104,104,0,1,0,232,128,104.2,104.2,0,0,0,128,24Zm24,144a8,8,0,0,1,0,16H104a7.3,7.3,0,0,1-2.5-.4A8,8,0,0,1,96,176a7.5,7.5,0,0,1,1.7-4.9l43.7-58.3A16,16,0,0,0,128,88a15.9,15.9,0,0,0-14.7,9.8,8,8,0,0,1-14.8-6.3,32,32,0,1,1,56,30.4l-.2.3L120,168Z"
                    fill="currentColor"
                  />
                </svg>
              </p>
            </div>

            <div class="benefit-text">
              <span class="fit-text1">생일 쿠폰 혜택</span> <br />
              <span class="fit-text2">전액 할인쿠폰 지급</span>
              <span class="fit-text2">* 유효기간 : 발급일로부터 7일</span>
            </div>

            <div class="benefit2-img">
              <img src="/public/images/promotion/be2.png" alt="benefit img 2" />
            </div>
          </div>

          <!-- benefit 3 -->
          <div class="benefit3">
            <div class="fit-title">
              <h1>Benefit</h1>
              <p>
                <svg class="emoji" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M128,24A104,104,0,1,0,232,128,104.2,104.2,0,0,0,128,24Zm21.5,153.5a36.2,36.2,0,0,1-51,0,8.1,8.1,0,0,1,11.4-11.4A19.9,19.9,0,1,0,124,132a8.1,8.1,0,0,1-7.1-4.3,8,8,0,0,1,.5-8.3L136.6,92H104a8,8,0,0,1,0-16h48a8.1,8.1,0,0,1,7.1,4.3,8,8,0,0,1-.5,8.3l-21.1,30a37.9,37.9,0,0,1,12,7.9,36.2,36.2,0,0,1,0,51Z"
                    fill="currentColor"
                  />
                </svg>
              </p>
            </div>

            <div class="benefit-text">
              <span class="fit-text1">친구 초대 혜택</span> <br />
              <span class="fit-text2">친구 초대 시 3,000원 혜택 <br />최대 3명까지 혜택 제공</span>
            </div>

            <div class="benefit3-img">
              <img src="/public/images/promotion/be3.png" alt="benefit img 3" />
            </div>
          </div>

          <!-- benefit4 -->
          <div class="benefit4">
            <div class="fit-title">
              <h1>Benefit</h1>
              <p>
                <svg class="emoji" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg">
                  <path
                    d="M128,24A104,104,0,1,0,232,128,104.2,104.2,0,0,0,128,24Zm28,152a8,8,0,0,1-16,0V152H100a8,8,0,0,1-6.5-3.4,7.9,7.9,0,0,1-1-7.3l24-68a8,8,0,0,1,15,5.4L111.3,136H140V112a8,8,0,0,1,16,0Z"
                    fill="currentColor"
                  />
                </svg>
              </p>
            </div>

            <div class="benefit-text">
              <span class="fit-text1">리뷰 작성 시 적립금</span> <br />
              <span class="fit-text2">텍스트 리뷰 : 500원, 사진 리뷰 : 1,000원</span>
            </div>

            <div class="benefit4-img">
              <img src="/images/promotion/be4.png" alt="benefit img 4" />
            </div>
          </div>
        </div>
      </div>

      <!-- ===== detail-benefit 1 ===== -->
      <div class="detail-benefit">
        <div class="detail-title">
          <h1>Benefit</h1>
          <svg class="emoji" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M128,24A104,104,0,1,0,232,128,104.2,104.2,0,0,0,128,24Zm12,152a8,8,0,0,1-16,0V98.9l-11.6,7.8a8,8,0,0,1-8.8-13.4l24-16a8.3,8.3,0,0,1,8.2-.4A8,8,0,0,1,140,84Z"
              fill="currentColor"
            />
          </svg>
        </div>
        <div class="detail-txt">
          <span class="detail-txt1">신규 회원 혜택</span>
          <span class="detail-txt2">신규 가입 최대 10,000원 혜택</span>
          <span class="detail-txt3">회원가입 시 적립금 &amp; 웰켐 쿠폰 자동 발급!</span>
        </div>

        <div class="pro-beimg">
          <div class="be-coupon">
            <img src="/images/promotion/bene1.png" alt="benefit1" />
            <img src="/images/promotion/bene3.png" alt="benefit1" />
            <p>쿠폰 사용기한 : 발급일로부터 10일 이내</p>
          </div>
        </div>
      </div>

      <!-- ===== detail-benefit 2 ===== -->
      <div class="detail-benefit">
        <div class="detail-title">
          <h1>Benefit</h1>
          <svg class="emoji" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M128,24A104,104,0,1,0,232,128,104.2,104.2,0,0,0,128,24Zm24,144a8,8,0,0,1,0,16H104a7.3,7.3,0,0,1-2.5-.4A8,8,0,0,1,96,176a7.5,7.5,0,0,1,1.7-4.9l43.7-58.3A16,16,0,0,0,128,88a15.9,15.9,0,0,0-14.7,9.8,8,8,0,0,1-14.8-6.3,32,32,0,1,1,56,30.4l-.2.3L120,168Z"
              fill="currentColor"
            />
          </svg>
        </div>
        <div class="detail-txt">
          <span class="detail-txt1">생일 쿠폰 혜택</span>
          <span class="detail-txt2">
            생일을 맞은 고객님께 감사의 마음 담아 <br />
            <strong>5,000원 쿠폰</strong>을 선물로 드립니다.
          </span>
        </div>

        <div class="pro-beimg">
          <div class="be-coupon">
            <img src="/images/promotion/bdcoupon.png" alt="benefit2" class="benefitimg2" />
            <p>쿠폰 사용기한 : 발급일로부터 7일 이내</p>
          </div>
        </div>
      </div>

      <!-- ===== detail-benefit 3 ===== -->
      <div class="detail-benefit">
        <div class="detail-title">
          <h1>Benefit</h1>
          <svg class="emoji" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M128,24A104,104,0,1,0,232,128,104.2,104.2,0,0,0,128,24Zm21.5,153.5a36.2,36.2,0,0,1-51,0,8.1,8.1,0,0,1,11.4-11.4A19.9,19.9,0,1,0,124,132a8.1,8.1,0,0,1-7.1-4.3,8,8,0,0,1,.5-8.3L136.6,92H104a8,8,0,0,1,0-16h48a8.1,8.1,0,0,1,7.1,4.3,8,8,0,0,1-.5,8.3l-21.1,30a37.9,37.9,0,0,1,12,7.9,36.2,36.2,0,0,1,0,51Z"
              fill="currentColor"
            />
          </svg>
        </div>
        <div class="detail-txt">
          <span class="detail-txt1">친구 초대 혜택</span>
          <span class="detail-txt2">친구에게 3천원 쿠폰 선물하고</span>
          <span class="detail-txt3">나도 3천원 쿠폰 받자 !</span>
        </div>

        <div class="pro-beimg">
          <div class="be-coupon">
            <img src="/images/promotion/bene3img.png" alt="benefit3" />
            <p>
              친구가 가입할 때 초대코드를 입력하고 첫 구매하면 <br />
              친구도 나도 <strong>3천원 쿠폰</strong>을 받을 수 있어요
            </p>
          </div>
        </div>
      </div>

      <!-- ===== detail-benefit 4 ===== -->
      <div class="detail-benefit">
        <div class="detail-title">
          <h1>Benefit</h1>
          <svg class="emoji" viewBox="0 0 256 256" xmlns="http://www.w3.org/2000/svg">
            <path
              d="M128,24A104,104,0,1,0,232,128,104.2,104.2,0,0,0,128,24Zm28,152a8,8,0,0,1-16,0V152H100a8,8,0,0,1-6.5-3.4,7.9,7.9,0,0,1-1-7.3l24-68a8,8,0,0,1,15,5.4L111.3,136H140V112a8,8,0,0,1,16,0Z"
              fill="currentColor"
            />
          </svg>
        </div>

        <div class="detail-txt">
          <span class="detail-txt1">리뷰 적립 혜택</span>
          <span class="detail-txt2">마타주를 사용하시고 생생한 리뷰를 남겨주세요.</span>
          <span class="detail-txt3">
            <strong>포토리뷰</strong>를 남겨주시는 분들께 <strong>적립금 1,000원</strong>을 드립니다.
          </span>
        </div>

        <div class="pro-beimg">
          <div class="review-grid">
            <figure class="review-card">
              <img src="/images/promotion/pro-review1.png" alt="포토 리뷰" />
              <figcaption>
                <span class="cap-title">포토 리뷰</span>
                <span class="cap-amount">적립금 1,000원</span>
              </figcaption>
            </figure>

            <figure class="review-card">
              <img src="/images/promotion/pro-review2.png" alt="텍스트 리뷰" />
              <figcaption>
                <span class="cap-title">텍스트 리뷰</span>
                <span class="cap-amount">적립금 500원</span>
              </figcaption>
            </figure>
          </div>

          <p class="review-note">리뷰나 관련없는 게시글이면 포인트는 지급되지 않습니다.</p>
        </div>
      </div>

      <!-- ===== app 섹션 ===== -->
      <div class="pro-app">
        <div class="app-img">
          <img src="/public/images/promotion/downloadimg.png" alt="app img" />
        </div>
        <div class="app-txt">
          <h2>
            마타주와 함께 보관하고<br />
            할인 혜택을 받아보세요
          </h2>
          <p>
            마타주 가입 시, 매달 쿠폰이 제공됩니다.<br />
            지금 바로 가입하지 않을 이유가 없어요.
          </p>
          <a href="#">
            <button>마타주 어플 다운 받기</button>
          </a>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup></script>

<style lang="scss" scoped>
@use "/src/assets/style/variables" as *;

/* ===================== 새 래퍼 ===================== */
.promo-page {
  position: relative;
  width: 100%;
  min-height: 100vh;
  overflow: hidden; /* 풍선이 약간 나가도 스크롤 안 생기게 */
  background: #fff;
}

/* ===================== 풍선 레이어 ===================== */
.floating-balloons {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 0; /* 컨텐츠보다 뒤로 */
}

/* 공통 풍선 모양 */
.balloon {
  position: absolute;
  width: clamp(78px, 8.3vw, 120px);
  aspect-ratio: 3 / 4;
  border-radius: 50% 50% 46% 46%;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.12);
  opacity: 0.85;
  animation: floatY 9s ease-in-out infinite;
  transform-origin: center bottom;
}

/* 끈 */
.balloon::after {
  content: "";
  position: absolute;
  bottom: -38px;
  left: 50%;
  width: 2px;
  height: 44px;
  transform: translateX(-50%);
  background: rgba(66, 140, 148, 0.35);
}

/* 색상: 다르게 */
.b1 {
  background: radial-gradient(circle at 30% 30%, #d7f5ff 0%, #8ecfe2 52%, #4da0a7 100%);
  top: 9vh;
  /* 화면 가운데에서 약간만 바깥으로 */
  left: clamp(0.6rem, 6vw, 4.3rem);
  animation-duration: 9.1s;
}
.b2 {
  background: radial-gradient(circle at 30% 30%, #ffe1df 0%, #ffaca6 55%, #ff756a 100%);
  top: 38vh;
  left: clamp(1.5rem, 7.4vw, 5.2rem);
  width: clamp(70px, 7.4vw, 108px);
  animation-duration: 10.4s;
  animation-delay: -2.7s;
}
.b3 {
  background: radial-gradient(circle at 30% 30%, #fff7d3 0%, #ffdf7d 52%, #ffd055 100%);
  top: 71vh;
  left: clamp(0.6rem, 5.8vw, 4.5rem);
  animation-duration: 11.2s;
  animation-delay: -1.5s;
}
.b4 {
  background: radial-gradient(circle at 30% 30%, #f6dfff 0%, #d7b5ff 45%, #ac82ff 100%);
  top: 16vh;
  right: clamp(0.8rem, 5.8vw, 4.1rem);
  animation-duration: 9.6s;
  animation-delay: -3.1s;
}
.b5 {
  background: radial-gradient(circle at 30% 30%, #c6efff 0%, #7dd3ff 50%, #3ca7d8 100%);
  top: 55vh;
  right: clamp(1.4rem, 7vw, 5.2rem);
  width: clamp(85px, 9.5vw, 130px);
  animation-duration: 10.7s;
  animation-delay: -1.2s;
}

/* 위아래 둥둥 + 살짝 회전 */
@keyframes floatY {
  0% {
    transform: translateY(0) rotate(0deg) scale(1);
  }
  25% {
    transform: translateY(-12px) rotate(1.6deg) scale(1.01);
  }
  50% {
    transform: translateY(6px) rotate(-1.4deg) scale(0.995);
  }
  75% {
    transform: translateY(-10px) rotate(1deg) scale(1.005);
  }
  100% {
    transform: translateY(0) rotate(0deg) scale(1);
  }
}

/* 1220 밑에서는 풍선 치우기 (너 무너지는 거 싫어하니까) */
@media (max-width: 1220px) {
  .floating-balloons {
    display: none;
  }
}

/* ===================== 기존 콘텐츠 ===================== */
.promo-inner {
  width: 100%;
  position: relative;
  z-index: 1; /* 풍선 위로 */
  background: transparent;
}

.promo-banner {
  position: relative;
  z-index: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100%;
  margin-top: 50px;
}

/* 네가 쓰던 통통 효과 */
.promo-banner img {
  animation: balloon-bounce 2.2s cubic-bezier(0.22, 0.61, 0.36, 1) infinite;
  transform-origin: 50% 100%;
  will-change: transform;
  backface-visibility: hidden;
}
.promo-banner:hover img {
  animation-duration: 1.6s;
}
@keyframes balloon-bounce {
  0% {
    transform: translateY(0) scale(1, 1);
  }
  20% {
    transform: translateY(-14px) scale(0.98, 1.02);
  }
  40% {
    transform: translateY(0) scale(1.04, 0.96);
  }
  60% {
    transform: translateY(-8px) scale(0.99, 1.01);
  }
  80% {
    transform: translateY(0) scale(1.02, 0.98);
  }
  100% {
    transform: translateY(0) scale(1, 1);
  }
}

.promo-benefit {
  text-align: center;
}

.promo-title h1 {
  color: #000;
  font-size: clamp(30px, 5vw, 50px);
  font-weight: 500;
  margin-top: 50px;
}
.promo-title h2 {
  color: #3e9c9b;
  font-size: clamp(30px, 5vw, 40px);
  font-weight: 700;
}
.promo-title p {
  color: #000;
  font-size: clamp(20px, 3vw, 28px);
  font-weight: 500;
  margin-bottom: 30px;
}

.benefit-list {
  display: grid;
  grid-template-columns: repeat(2, 300px);
  gap: 13px;
  justify-content: center;
  align-items: stretch;
  margin: 30px auto;
  max-width: 700px;
}

.benefit1,
.benefit2,
.benefit3,
.benefit4 {
  width: 300px;
  min-height: 320px;
  border: 1px solid #e0e0e0;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  padding: 26px 16px;
  gap: 8px;
  background: #fff;
}

.fit-title {
  display: flex;
  flex-direction: column;
  align-items: center;
  margin-top: 8px;
}
.fit-title > h1 {
  font-size: $title-sm;
  color: #3e9c9b;
  font-weight: 600;
}
.emoji {
  color: #3e9c9b;
  width: 28px;
  height: 28px;
  display: block;
}

.benefit-text {
  display: flex;
  flex-direction: column;
  align-items: center;
  line-height: 0.5;
}
.benefit-text > .fit-text1 {
  font-size: $text-md;
  font-weight: 700;
  margin: 6px 0 2px;
  color: #000;
}
.benefit-text > .fit-text2 {
  font-size: 11px;
  color: #a0a0a0;
  line-height: 1.5;
}

.benefit1-img,
.benefit2-img,
.benefit3-img,
.benefit4-img {
  width: 100%;
  display: flex;
  justify-content: center;
  margin-top: auto;
}
.benefit1-img img,
.benefit2-img img,
.benefit3-img img,
.benefit4-img img {
  max-width: 130px;
  height: auto;
  display: block;
}

.detail-benefit {
  width: 50%;
  max-width: 710px;
  height: auto;
  margin: 80px auto;
  padding: 70px 24px;
  border: 1px solid #e0e0e0;
  background: #fff;
  text-align: center;
}
.detail-title {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 5px;
  margin-bottom: 16px;
}
.detail-title > h1 {
  font-size: $quote;
  font-weight: 400;
  color: #3e9c9b;
}
.detail-title .emoji {
  color: #3e9c9b;
  width: 28px;
  height: 28px;
}

.detail-txt {
  display: flex;
  flex-direction: column;
  gap: 8px;
  margin-bottom: 30px;
}
.detail-txt span:first-child {
  color: #000;
  font-size: $title-lg;
  font-weight: 700;
  line-height: 1.3;
  margin-bottom: 12px;
}
.detail-txt > .detail-txt2 {
  font-size: $quote;
  color: #000;
  font-weight: 600;
}
.detail-txt span:last-child {
  font-size: $quote;
  color: #000;
  font-weight: 400;
}

.be-coupon {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 12px;
  margin: auto;
  width: 65%;
}
.be-coupon img {
  max-width: 390px;
  width: 70%;
  object-fit: contain;
  margin: auto;
}
.be-coupon img.benefitimg2 {
  width: 500px;
  height: auto;
  max-width: 100%;
  margin: 0 auto;
}
.be-coupon > p {
  font-size: 12px;
  color: #555353;
  font-weight: 500;
  text-align: center;
}

.detail-benefit:nth-of-type(2) {
  margin-top: 120px;
}
.detail-benefit:nth-of-type(3) {
  margin-top: 80px;
}

.detail-benefit:nth-of-type(3) strong {
  color: #3e9c9b;
  font-weight: 700;
}
.detail-benefit:nth-of-type(3) p {
  font-size: 12px;
}
.detail-benefit:nth-of-type(3) .be-coupon {
  width: 100%;
}

.promo-inner > .detail-benefit:nth-of-type(4) .detail-txt2 {
  font-size: 20px;
  font-weight: 400;
  color: #000;
}
.promo-inner > .detail-benefit:nth-of-type(4) .detail-txt3 {
  font-size: clamp(30px, 2vw, 38px);
  font-weight: 700;
  color: #3e9c9b;
  line-height: 1.2;
}
.promo-inner > .detail-benefit:nth-of-type(4) p {
  color: #555353;
  font-weight: 500;
  font-size: 13px;
}
.promo-inner > .detail-benefit:nth-of-type(4) strong {
  color: #3e9c9b;
}

.promo-inner > .detail-benefit:nth-of-type(5) .detail-txt2 {
  font-size: clamp(14px, 2vw, 16px);
  font-weight: 400;
}
.promo-inner > .detail-benefit:nth-of-type(5) .detail-txt3 {
  font-size: clamp(13px, 2vw, 16px);
}
.promo-inner > .detail-benefit:nth-of-type(5) strong {
  color: #3e9c9b;
}
.review-grid {
  width: 100%;
  display: flex;
  justify-content: center;
  gap: 20px;
  margin: 0 auto 16px;
}
.review-card {
  width: 230px;
  background: #fbfbfb;
  border-radius: 16px;
  padding: 15px;
  text-align: center;
}
.review-card img {
  width: 100%;
  max-width: 180px;
  height: auto;
  display: block;
  margin: 0 auto 10px;
  object-fit: contain;
}
.review-card .cap-title {
  display: block;
  font-size: 18px;
  font-weight: 600;
  color: #000;
}
.review-card .cap-amount {
  display: block;
  font-size: 18px;
  font-weight: 700;
  color: #3e9c9b;
}
.review-note {
  font-size: 14px;
  color: #b8b3b3;
  text-align: center;
  margin-top: 20px;
}

.pro-app {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 40px;
  max-width: 900px;
  margin: 80px auto;
  padding: 25px 129px;
  flex-wrap: wrap;
}
.app-img img {
  width: 320px;
  height: auto;
  object-fit: cover;
}
.app-txt {
  flex: 1;
  min-width: 280px;
  text-align: left;
}
.app-txt h2 {
  font-size: 30px;
  font-weight: 700;
  color: #000000;
  margin-bottom: 20px;
  line-height: 1.2;
}
.app-txt p {
  font-size: 15px;
  color: #b8b3b3;
  white-space: nowrap;
  margin-bottom: 20px;
}
.app-txt button {
  background-color: #3e9c9b;
  color: #fff;
  font-size: 14px;
  font-weight: 500;
  text-align: center;
  border: none;
  border-radius: 9999px;
  padding: 10px 20px;
  cursor: pointer;
  transition: background 0.2s ease, transform 0.15s ease;
}
.app-txt button:hover {
  background-color: #3A8C88;
  transform: translateY(1px);
}

/* ===================== 반응형 ===================== */
@media (max-width: 1024px) {
  .detail-benefit {
    width: 70%;
    padding: 48px 20px;
  }
}

@media (max-width: 680px) {
  .promo-title h1 {
    font-size: 40px;
    line-height: 0.9;
  }
  .promo-title h2 {
    font-size: 40px;
  }
  .promo-title p {
    font-size: 23px;
  }

  .detail-benefit {
    width: min(92%, 640px);
    padding: 28px 20px;
    margin: -5px auto 50px;
  }
  .detail-title > h1 {
    font-size: clamp(18px, 2.8vw, 22px);
  }

  .detail-txt .detail-txt1 {
    font-size: clamp(26px, 4.8vw, 32px);
  }
  .detail-txt .detail-txt2,
  .detail-txt .detail-txt3 {
    font-size: clamp(14px, 2.6vw, 18px);
    line-height: 1.4;
  }

  .be-coupon {
    width: min(86%, 360px);
    gap: 10px;
  }
  .be-coupon img {
    width: 70%;
    height: auto;
  }
  .be-coupon img.benefitimg2 {
    width: 90%;
    height: auto;
    margin: 0 auto;
  }
  .pro-app {
    padding: 25px 180px;
  }
}

@media (max-width: 600px) {
  .benefit-list {
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 10px;
    max-width: 94vw;
  }
  .benefit1,
  .benefit2,
  .benefit3,
  .benefit4 {
    width: 100%;
    min-height: 0;
    padding: clamp(12px, 3vw, 16px) clamp(10px, 2.5vw, 14px);
    gap: clamp(6px, 1.8vw, 8px);
  }
  .fit-title > h1 {
    font-size: clamp(16px, 3.6vw, 18px);
  }
  .emoji {
    width: clamp(20px, 5vw, 24px);
    height: clamp(20px, 5vw, 24px);
  }
  .benefit-text > .fit-text1 {
    font-size: clamp(14px, 3.6vw, 16px);
  }
  .benefit-text > .fit-text2 {
    font-size: clamp(10px, 3vw, 12px);
  }

  .benefit1-img img,
  .benefit2-img img,
  .benefit3-img img,
  .benefit4-img img {
    width: clamp(80px, 28vw, 120px);
    height: auto;
  }

  .pro-app {
    padding: 25px 140px;
  }
}

@media (max-width: 768px) {
  .benefit-list {
    grid-template-columns: 1fr;
    gap: 16px;
    max-width: 92vw;
  }
  .benefit1,
  .benefit2,
  .benefit3,
  .benefit4 {
    width: 100%;
    padding: 16px;
  }
  .fit-title > h1 {
    font-size: clamp(18px, 5vw, 20px);
  }
  .benefit-text > .fit-text1 {
    font-size: clamp(15px, 4.5vw, 16px);
  }
  .benefit-text > .fit-text2 {
    font-size: clamp(10px, 3.5vw, 12px);
  }

  .detail-benefit {
    width: 92%;
    padding: 24px 16px;
  }
  .review-grid {
    grid-template-columns: 1fr;
  }
  .pro-app {
    padding: 25px 224px;
  }
}

@media (max-width: 768px) and (min-width: 601px) {
  .benefit-list {
    grid-template-columns: repeat(2, minmax(240px, 1fr));
    gap: 14px;
    max-width: 92vw;
  }

  .benefit1,
  .benefit2,
  .benefit3,
  .benefit4 {
    width: 100%;
    min-height: 0;
    padding: 20px 16px;
    gap: 8px;
  }

  .detail-benefit {
    width: 86%;
    padding: 40px 20px;
    margin: 60px auto 80px;
  }

  .review-grid {
    grid-template-columns: 1fr 1fr;
    gap: 16px;
  }
}

@media (max-width: 390px) {
    .promo-banner img {
    width: min(92vw, 360px);
    height: auto;
    object-fit: contain;
  }
  .app-img {
    width: 280px;
    text-align: left;
  }
  .app-img img {
    width: 280px;
  }
  .app-txt h2 {
    font-size: 28px;
  }
  .app-txt p {
    font-size: 13px;
  }
  .app-txt button {
    font-size: 13px;
    padding: 10px 15px;
  }
  .cap-amount {
    font-size: 17.5px;
  }
  .review-note{
    font-size: 13.5px;
  }
  .pro-app{
    padding: 25px 195px;
  }
  .app-txt button{font-weight: 600;}  // 머지해보고 필요없음 지우기

}

/* ===================== 풍선 레이어 ===================== */
.floating-balloons {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 0;
}

/* 공통 풍선 모양 */
.balloon {
  position: absolute;
  width: clamp(74px, 8vw, 118px);
  aspect-ratio: 3 / 4;
  border-radius: 50% 50% 46% 46%;
  box-shadow: 0 12px 30px rgba(0, 0, 0, 0.12);
  opacity: 0.9;
  animation: floatY 9s ease-in-out infinite;
  transform-origin: center bottom;
}

/* 끈 */
.balloon::after {
  content: "";
  position: absolute;
  bottom: -38px;
  left: 50%;
  width: 2px;
  height: 44px;
  transform: translateX(-50%);
  background: rgba(66, 140, 148, 0.35);
}

/* ===== 개별 풍선 (위치+색 제각각) ===== */

/* ===== 개별 풍선 (위치 그대로, 색만 진하게) ===== */

/* 왼쪽 1 */
.b1 {
  top: 16vh;
  left: 450px;
  /* 밝은 민트 → 채도/명도 조금 내려서 더 진하게 */
  background: radial-gradient(circle at 30% 30%, #b8ecff 0%, #69bfd5 52%, #2f8990 100%);
  animation-duration: 9.5s;
  animation-delay: -1.2s;
}

/* 왼쪽 2 (코랄) */
.b2 {
  top: 45vh;
  left: 200px;
  width: clamp(68px, 7.3vw, 105px);
  background: radial-gradient(circle at 30% 30%, #ffd2cd 0%, #ff8e82 52%, #f85d4f 100%);
  animation-duration: 10.8s;
  animation-delay: -3.1s;
}

/* 왼쪽 3 (옐로) */
.b3 {
  top: 8vh;
  left: clamp(0.8rem, 3.5vw, 2.4rem);
  background: radial-gradient(circle at 30% 30%, #ffefb7 0%, #ffcf5c 50%, #f3b028 100%);
  animation-duration: 11.3s;
  animation-delay: -2.2s;
}

/* 오른쪽 1 (보라) */
.b4 {
  top: 10vh;
  right: 400px;
  background: radial-gradient(circle at 30% 30%, #f0d2ff 0%, #c694ff 48%, #9657ff 100%);
  animation-duration: 9.9s;
  animation-delay: -4s;
}

/* 오른쪽 2 (하늘) */
.b5 {
  top: 30vh;
  right: clamp(2.7rem, 6.2vw, 4.8rem);
  width: clamp(82px, 9vw, 132px);
  background: radial-gradient(circle at 30% 30%, #bde7ff 0%, #62b7ff 48%, #1f87c7 100%);
  animation-duration: 10.6s;
  animation-delay: -1.7s;
}

/* 오른쪽 3 (핑크) */
.b6 {
  top: 45vh;
  right: 400px;
  width: clamp(70px, 7.1vw, 110px);
  background: radial-gradient(circle at 30% 30%, #ffd2e2 0%, #ff8cb7 45%, #f5568f 100%);
  animation-duration: 12.1s;
  animation-delay: -2.9s;
}

/* 떠다니는 모션 (그대로) */
@keyframes floatY {
  0% {
    transform: translateY(0) rotate(0deg) scale(1);
  }
  25% {
    transform: translateY(-13px) rotate(1.7deg) scale(1.01);
  }
  50% {
    transform: translateY(5px) rotate(-1.6deg) scale(0.995);
  }
  75% {
    transform: translateY(-11px) rotate(1deg) scale(1.004);
  }
  100% {
    transform: translateY(0) rotate(0deg) scale(1);
  }
}

/* 1220 밑에서는 숨김 */
@media (max-width: 1220px) {
  .floating-balloons {
    display: none;
  }
}
/* ===================== 풍선 레이어 ===================== */
.floating-balloons {
  position: absolute;      /* ← fixed 아니고 */
  top: 0;
  left: 0;
  right: 0;
  height: 1100px;          /* 풍선 보일 구간, 필요하면 900~1200px 사이로 조절 */
  pointer-events: none;
  z-index: 0;
  overflow: hidden;        /* 아래로 튀어나오는 줄 숨김 */
}


</style>
