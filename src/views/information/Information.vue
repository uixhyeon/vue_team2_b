<template>
  <div class="info-inner">
    <section class="info-guide">
      <div class="info-head">
        <h2 class="info-title">이용 안내</h2>
        <p class="info-sub">무인보관함 이용 방법과 배송은 어떻게 신청하나요?</p>
      </div>

      <ol class="info-steps">
        <!-- 01 -->
        <li class="step-card">
          <div class="step-head">
            <span class="step-first">01</span>
            <span class="step-second">사물함 예약</span>
          </div>
          <hr class="step-line" />
          <p class="step-txt">원하는 지점과 <br />사물함 크기, <br />이용 날짜를 선택합니다.</p>
          <!-- 네비 -->
          <div class="step-icon">
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path d="M12 2a7 7 0 0 0-7 7c0 5 7 13 7 13s7-8 7-13a7 7 0 0 0-7-7Zm0 9a2 2 0 1 1 0-4 2 2 0 0 1 0 4Z" />
            </svg>
          </div>
        </li>

        <!-- 화살표 -->
        <div class="info-arrow">
          <svg viewBox="0 0 24 24" aria-hidden="true">
            <path
              d="M8 4l8 8-8 8"
              fill="none"
              stroke="#3e9c9b"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round" />
          </svg>
        </div>

        <!-- 02 -->
        <li class="step-card">
          <div class="step-head">
            <span class="step-first">02</span>
            <span class="step-second">배송 여부 선택</span>
          </div>
          <hr class="step-line" />
          <p class="step-txt">원하는 배송 방식을 <br />선택하세요.</p>
          <!-- 박스 -->
          <div class="step-icon">
            <svg viewBox="0 0 20 20" aria-hidden="true">
              <path
                d="M10.0001 7.96137L13.0289 6.74988L5.52875 3.74984L2.94291 4.78418C2.81019 4.83726 2.68812 4.90794 2.57894 4.99288L10.0001 7.96137ZM2.03542 5.85251C2.01215 5.9576 2 6.06624 2 6.17689V13.8228C2 14.4362 2.37343 14.9877 2.94291 15.2155L8.70013 17.5184C8.95942 17.6221 9.22802 17.6936 9.50015 17.7329V8.8384L2.03542 5.85251ZM10.5001 17.7328C10.7722 17.6936 11.0407 17.6221 11.2999 17.5184L17.0571 15.2155C17.6266 14.9877 18 14.4362 18 13.8228V6.17689C18 6.06628 17.9879 5.95767 17.9646 5.85262L10.5001 8.8384V17.7328ZM17.4212 4.99296L14.3751 6.21137L6.87504 3.21132L8.70013 2.48129C9.53457 2.14751 10.4654 2.14751 11.2999 2.48129L17.0571 4.78418C17.1898 4.83728 17.312 4.90798 17.4212 4.99296Z" />
            </svg>
          </div>
        </li>

        <!-- 화살표 -->
        <div class="info-arrow">
          <svg viewBox="0 0 24 24" aria-hidden="true">
            <path
              d="M8 4l8 8-8 8"
              fill="none"
              stroke="#3e9c9b"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round" />
          </svg>
        </div>

        <!-- 03 -->
        <li class="step-card">
          <div class="step-head">
            <span class="step-first">03</span>
            <span class="step-second">결제</span>
          </div>
          <hr class="step-line" />
          <p class="step-txt">예약 확인 후 결제합니다.</p>
          <!-- 카드 -->
          <div class="step-icon">
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path d="M20,3H4C1.8,3,0,4.8,0,7v1c0,0.6,0.4,1,1,1h22c0.6,0,1-0.4,1-1V7C24,4.8,22.2,3,20,3z" />
              <path
                d="M23,11H1c-0.6,0-1,0.4-1,1v5c0,2.2,1.8,4,4,4h16c2.2,0,4-1.8,4-4v-5C24,11.4,23.6,11,23,11z M11,18H5c-0.6,0-1-0.4-1-1    s0.4-1,1-1h6c0.6,0,1,0.4,1,1S11.6,18,11,18z M19,15h-2c-0.6,0-1-0.4-1-1s0.4-1,1-1h2c0.6,0,1,0.4,1,1S19.6,15,19,15z" />
            </svg>
          </div>
        </li>

        <!-- 화살표 -->
        <div class="info-arrow">
          <svg viewBox="0 0 24 24" aria-hidden="true">
            <path
              d="M8 4l8 8-8 8"
              fill="none"
              stroke="#028587"
              stroke-width="2"
              stroke-linecap="round"
              stroke-linejoin="round" />
          </svg>
        </div>

        <!-- 04 -->
        <li class="step-card">
          <div class="step-head">
            <span class="step-first">04</span>
            <span class="step-second">지점 방문</span>
          </div>
          <hr class="step-line" />
          <p class="step-txt">지점에 방문해 <br />짐을 보관하세요.</p>
          <!-- 지점 -->
          <div class="step-icon">
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path
                d="M6.75 2C5.50736 2 4.5 3.00736 4.5 4.25V20.75C4.5 21.1642 4.83579 21.5 5.25 21.5H7.5V17.25C7.5 16.8358 7.83579 16.5 8.25 16.5H15.75C16.1642 16.5 16.5 16.8358 16.5 17.25V21.5H18.75C19.1642 21.5 19.5 21.1642 19.5 20.75V11.7493C19.5 10.5067 18.4926 9.4993 17.25 9.4993H16.5V4.25C16.5 3.00736 15.4926 2 14.25 2H6.75ZM7.5 6.5C7.5 5.94772 7.94772 5.5 8.5 5.5C9.05228 5.5 9.5 5.94772 9.5 6.5C9.5 7.05228 9.05228 7.5 8.5 7.5C7.94772 7.5 7.5 7.05228 7.5 6.5ZM8.5 12.5C9.05228 12.5 9.5 12.9477 9.5 13.5C9.5 14.0523 9.05228 14.5 8.5 14.5C7.94772 14.5 7.5 14.0523 7.5 13.5C7.5 12.9477 7.94772 12.5 8.5 12.5ZM7.5 10C7.5 9.44772 7.94772 9 8.5 9C9.05228 9 9.5 9.44772 9.5 10C9.5 10.5523 9.05228 11 8.5 11C7.94772 11 7.5 10.5523 7.5 10ZM12 5.5C12.5523 5.5 13 5.94772 13 6.5C13 7.05228 12.5523 7.5 12 7.5C11.4477 7.5 11 7.05228 11 6.5C11 5.94772 11.4477 5.5 12 5.5ZM11 13.5C11 12.9477 11.4477 12.5 12 12.5C12.5523 12.5 13 12.9477 13 13.5C13 14.0523 12.5523 14.5 12 14.5C11.4477 14.5 11 14.0523 11 13.5ZM15.5 12.5C16.0523 12.5 16.5 12.9477 16.5 13.5C16.5 14.0523 16.0523 14.5 15.5 14.5C14.9477 14.5 14.5 14.0523 14.5 13.5C14.5 12.9477 14.9477 12.5 15.5 12.5ZM11 10C11 9.44772 11.4477 9 12 9C12.5523 9 13 9.44772 13 10C13 10.5523 12.5523 11 12 11C11.4477 11 11 10.5523 11 10Z"
                fill="#fff" />
              <path d="M15 21.5V18H12.7499V21.5H15Z" fill="#fff" />
              <path d="M11.2499 21.5V18H9V21.5H11.2499Z" fill="#fff" />
            </svg>
          </div>
        </li>
      </ol>
    </section>
  </div>
  <Information2 />
  <Information3 />
</template>

<script setup>
import Information2 from "./Information2.vue";
import Information3 from "./Information3.vue";
</script>

<style scoped>
.info-inner {
  box-sizing: border-box;
  max-width: 1100px;
  width: 100%;
  margin: 0 auto;
}

.info-guide {
  text-align: left;
  margin: 80px auto;
}

.info-head {
  margin-bottom: 40px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  width: 100%;
}

.info-title {
  font-size: 32px;
  font-weight: 800;
  color: #000;
}

.info-sub {
  font-size: 18px;
  color: #555353;
}
.step-head {
  display: flex;
  flex-direction: column;
  text-align: left;
}
.step-first {
  color: #3e9c9b;
  font-weight: 600;
  font-size: 16px;
  margin-bottom: 2px;
}
.step-second {
  color: #3e9c9b;
  font-weight: 700;
  font-size: 18px;
}

.step-line {
  width: 100%;
  border: none;
  border-top: 1px solid #e0e0e0;
  margin: 15px 0 15px;
}
.step-txt {
  color: #000;
  font-size: 16px;
  line-height: 1.5;
  text-align: left;
  align-self: flex-start;
  width: 100%;
  min-height: 60px;
}
.info-steps {
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 20px;
  flex-wrap: wrap;
  list-style: none;
}

.step-card {
  background: #fff;
  border: 1px solid #e0e0e0;
  border-radius: 16px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
  padding: 24px 20px;
  width: 200px;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  text-align: left;
  position: relative;
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.step-icon {
  width: 50px;
  height: 50px;
  background-color: #3e9c9b;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 16px;
  margin-left: auto;
}

.step-icon svg {
  width: 35px;
  height: 35px;
  fill: #ffffff;
}
/* 화살표 스타일 */
.info-arrow {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-shrink: 0;
}

.info-arrow svg {
  width: 22px;
  height: 22px;
  fill: none;
  stroke: #3e9c9b;
  stroke-width: 2;
  stroke-linecap: round;
  stroke-linejoin: round;
}

/* 반응형 */
@media (max-width: 1290px) {
  .info-inner {
    padding: 0 20px;
  }
  .info-title {
    font-size: 30px;
  }
  .info-sub {
    font-size: 17px;
  }

  .info-steps {
    gap: 27px;
  }
  .step-card {
    width: 188px;
    padding: 20px 16px;
    border-radius: 14px;
  }
  .step-icon {
    width: 46px;
    height: 46px;
  }
  .step-icon svg {
    width: 30px;
    height: 30px;
  }
  .info-arrow svg {
    width: 20px;
    height: 20px;
  }
}
@media (max-width: 1100px) {
  .info-inner {
    padding: 0 30px;
  }

  .info-steps {
    flex-wrap: nowrap;
    gap: 20px;
    list-style: none;
    align-items: stretch;
  }
  .step-card {
    padding: 14px 12px;
    border-radius: 12px;
  }
  .step-line {
    margin: 15px 0;
  }
  .step-txt {
    line-height: 1.45;
    min-height: auto;
    flex-grow: 1;
  }

  .step-icon {
    width: 36px;
    height: 36px;
    flex-shrink: 0;
  }
  .step-icon svg {
    width: 20px;
    height: 20px;
  }

  .info-arrow svg {
    width: 20px;
    height: 20px;
    stroke-width: 2;
  }
  .info-title {
    font-size: 26px;
  }
  .info-sub {
    font-size: 15px;
  }
}
@media (max-width: 1000px)  {
  .info-inner {
    padding: 0 20px;
  }

  .info-steps {
    list-style: none;
    gap: 8px;
  }
  .info-sub {
    font-size: 16px;
  }
  .info-steps .step-card {
    max-width: none;
    padding: 18px 16px;
    border-radius: 14px;
    box-sizing: border-box;
  }
  .info-steps .info-arrow {
    display: flex;
    align-items: center;
    justify-content: center;
  }
  .info-steps .info-arrow svg {
    width: 20px;
    height: 20px;
    stroke: #3e9c9b;
  }

  .step-first {
    font-size: 16px;
  }
  .step-second {
    font-size: 18px;
  }
  .step-line {
    margin: 10px 0;
  }
  .step-txt {
    font-size: 16px;
    line-height: 1.45;
    min-height: auto;
  }
  .step-icon {
    width: 36px;
    height: 36px;
    margin-top: 10px;
  }
  .step-icon svg {
    width: 24px;
    height: 24px;
  }
}
@media (max-width: 800px) {
  .step-second{font-size: 17px;}
  .step-txt {
    font-size: 15px;
  }
}
@media (max-width: 767px) {
  .info-steps {
    display: grid;
    grid-template-columns: 1fr 1 1fr;
    gap:8px;
    justify-content: center;
    justify-items: center; 
    align-items: stretch;
  }
  .info-steps > :nth-child(4), .info-steps > :nth-child(6){
    display: none;
  }

  /* 1행 */
  .info-steps > :nth-child(1) {
    grid-column: 1;
    grid-row: 1;
  } /* 01 */
  .info-steps > :nth-child(2) {
    grid-column: 3;
    grid-row: 1;
  } /* → */
  .info-steps > :nth-child(3) {
    grid-column: 3;
    grid-row: 1;
  } /* 02 */

  /* 2행 */
  .info-steps > :nth-child(5) {
    grid-column: 1;
    grid-row: 2;
  } /* 03 */
  /* 6번 숨겼으니까 7번을 오른쪽 카드로 */
  .info-steps > :nth-child(7) {
    grid-column: 3;
    grid-row: 2;
  } /* 04 */

  /* 카드들 높이 맞추기 */
  .info-steps .step-card {
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 16px 14px;
    border-radius: 14px;
    box-sizing: border-box;
  }

  /* 화살표는 가운데만 */
  .info-steps .info-arrow {
    display: flex;
    align-items: center;
    justify-content: center;
  }

  /* 텍스트 살짝 압축 */
  .step-first,
  .step-second {
    font-size: 13px;
  }
  .step-txt {
    font-size: 12.5px;
    line-height: 1.45;
    flex: 1 1 auto;
  }
  .step-icon {
    width: 34px;
    height: 34px;
    margin-top: 10px;
    margin-left: auto;
  }
  .step-icon svg {
    width: 22px;
    height: 22px;
  }
}
@media (max-width: 440px) and (min-width:300px) {
  .info-steps {
    display: flex;
    flex-direction: column; 
    gap: 10px;
    align-items: center;
    justify-content: center;
  }

  .info-steps .info-arrow {
    display: none;
  }

  .info-steps .step-card {
    width: 50%;
    max-width: 350px;
    height: auto;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    padding: 16px 14px;
    border-radius: 14px;
    box-sizing: border-box;
  }

  .step-first,
  .step-second {
    font-size: 15px;
  }

  .step-txt {
    font-size: 13px;
    line-height: 1.45;
  }

  .step-icon {
    width: 34px;
    height: 34px;
    margin-top: 10px;
    margin-left: auto;
  }

  .step-icon svg {
    width: 25px;
    height: 25px;
  }
}


</style>
