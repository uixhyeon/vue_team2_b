<template>
  <div class="reserve-page">
    <Stepper :current-step="2" />

    <div class="content">
      <!-- 왼쪽: 정보 + 요약 -->
      <section class="left-section">
        <!-- 정보 카드 -->
        <div class="form_card">
          <div class="card_header"><h2>정보</h2></div>
          <div class="card_content">
            <table>
              <tbody>
                <tr v-if="form.name"><td>이름</td><td>{{ form.name }}</td></tr>
                <tr v-if="form.phone"><td>휴대폰</td><td>{{ form.phone }}</td></tr>
                <tr v-if="form.size"><td>사물함</td><td>{{ form.size }}</td></tr>
                <tr v-if="form.address"><td>주소</td><td>{{ form.address }}</td></tr>
                <tr v-if="form.dateRange && form.dateRange[0] && form.dateRange[1]">
                  <td>기간</td><td>{{ form.dateRange[0] }} ~ {{ form.dateRange[1] }}</td>
                </tr>
                <tr v-if="form.pickupAddress"><td>픽업 주소</td><td>{{ form.pickupAddress }}</td></tr>
                <tr v-if="form.homeAddress"><td>배송 주소</td><td>{{ form.homeAddress }}</td></tr>
                <tr class="total" v-if="totalPrice > 0">
                  <td>결제금액</td><td><strong>{{ formatKrw(totalPrice) }}</strong></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- 요약 카드 -->
        <div class="summary_card">
          <h2 class="card_title">선택 상품 요약</h2>
          <ul>
            <li v-for="tab in selectedTabs" :key="tab">{{ tab }} — {{ formatKrw(prices[tab]) }}</li>
          </ul>
          <div class="divider"></div>
          <div class="benefits">
            <div class="benefit-row">
              <label><input type="checkbox" v-model="useCoupon" /> Welcome 쿠폰 - 3,000원</label>
              <span v-if="useCoupon" class="muted">-{{ formatKrw(3000) }}</span>
            </div>
            <div class="benefit-row">
              <label><input type="checkbox" v-model="usePoints" /> 포인트 - 2,500P</label>
              <span v-if="usePoints" class="muted">-{{ formatKrw(2500) }}</span>
            </div>
          </div>
          <div class="total-row">
            <span>총 결제금액</span><strong>{{ formatKrw(finalTotal) }}</strong>
          </div>
        </div>
      </section>

      <!-- 오른쪽: 결제 방법 -->
      <section class="right-section">
        <div class="payment_card">
          <div class="card_header"><h3>결제방법 선택</h3></div>
          <div class="card_content">
            <div class="pay-grid">
              <button
                v-for="method in paymentMethods"
                :key="method.id"
                class="pay-card"
                :class="{ selected: selectedPayment === method.id }"
                @click="selectedPayment = method.id"
              >
                <span class="icon">  <img :src="method.img" :alt="method.label" class="pay-img" /></span>
                <span>{{ method.label }}</span>
              </button>
            </div>
          </div>
        </div>

        <button class="submit_btn" @click="saveAndPay">결제하기</button>
      </section>
    </div>
  </div>

 
</template>

<script setup>
import { ref, computed } from "vue";
import { useRouter, useRoute } from "vue-router";
import Stepper from "@/components/reserv/Stepper.vue";

const route = useRoute();
const router = useRouter();

/* ✅ 전달받은 form 데이터 */
const form = ref(
  route.query.form
    ? JSON.parse(route.query.form)
    : { name: "", phone: "", size: "", address: "", dateRange: [] }
);

/* ✅ 가격 정보 */
const baseTotal = ref(Number(route.query.totalPrice || 0));
const selectedTabs = ref(["사물함 예약"]);
const prices = {
  "사물함 예약": baseTotal.value,
  "짐 가져오기": 15000,
  "집으로 배송하기": 20000,
};

/* ✅ 쿠폰·포인트 */
const useCoupon = ref(false);
const usePoints = ref(false);

const totalPrice = computed(() =>
  selectedTabs.value.reduce((sum, tab) => sum + prices[tab], 0)
);

const discountAmount = computed(() => {
  let discount = 0;
  if (useCoupon.value) discount += 3000;
  if (usePoints.value) discount += 2500;
  return Math.min(discount, totalPrice.value);
});

const finalTotal = computed(() =>
  Math.max(totalPrice.value - discountAmount.value, 0)
);

/* ✅ 결제 수단 */
const selectedPayment = ref("card");
const paymentMethods = [
  { id: "card", label: "신용카드", img: "/images/reservation/image490.png" },
  { id: "kakao", label: "카카오페이", img: "/images/reservation/image493.png" },
  { id: "naver", label: "네이버페이", img: "/images/reservation/image491.png" },
  { id: "bank", label: "무통장입금", img: "/images/reservation/image492.png" },
];

/* ✅ 통화 포맷 */
const formatKrw = (v) =>
  new Intl.NumberFormat("ko-KR", { style: "currency", currency: "KRW" }).format(v);

/* ✅ 결제 함수 */
const saveAndPay = () => {
  alert(`✅ 결제가 완료되었습니다!
결제수단: ${selectedPayment.value}
결제금액: ${formatKrw(finalTotal.value)}`);

  const query = {
    form: JSON.stringify(form.value),
    total: finalTotal.value,
    payment: selectedPayment.value,
  };
  router.push({ path: "/reservation3", query });
};
</script>
<style lang="scss" scoped>
@use "/src/assets/style/variables" as *;

/* ✅ 기본 구조 */
.reserve-page {
  background: #f5f7f7;
  min-height: 100vh;
padding: 40px 0 80px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.content {
  width: 100%;
  max-width: 1120px;
  display: grid;
  grid-template-columns: 1fr;
  gap: 2.5rem;
  align-items: start;
padding: 20px;
@media (max-width: 1020px){
  width: 90%;
}
  @media (max-width: 500px) {
    grid-template-columns: 1fr;
   width: 100%;
  }
}
// ======왼오나뉨=====
.left-section{
 width: 100%;
 display: grid;
grid-template-columns: 1fr 1fr;
gap: 2.5rem;
  @media (max-width: 500px){
     grid-template-columns: 1fr;
  }
}

/* ✅ 공통 카드 스타일 */
.form_card,
.summary_card,
.payment_card {
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  border: 1px solid #f0f0f0;
  padding: 30px 40px;
  position: relative;
  transition: 0.3s ease;

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 12px;
    background: $color_main;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }

  @media (max-width: 1024px) {
    width: 100%;
    padding: 28px 32px;
  }

  @media (max-width: 600px) {
    padding: 24px 20px;
  }
}
.form_card{
  h2{
    font-size: $text-md;
  }
}
/* ✅ 테이블 */
.form_card table {
  width: 100%;
  border-collapse: collapse;
  
  td {
    padding: 6px 0;
    font-size: $text-sm;
    color: #333;
  }
  td:first-child {
    color: #777;
    font-weight: 500;
    width: 40%;
  }
  tr.total {
    border-top: 1px solid #ddd;
    td:last-child {
      color: $color_main;
      font-weight: 700;
    }
  }
}

/* ✅ 요약 카드 */
.summary_card {
  h2{
    font-size: $text-md;
  }
 .total-row{
    font-size: $text-md;
  }
  .card_title {
    font-size: $text-md;
    font-weight: 700;
    margin-bottom: 1rem;
  }
  ul {
    margin-bottom: 1.2rem;
    li {
      font-size: $text-sm;
      color: #333;
      line-height: 1.6;
    }
  }

  .divider {
    height: 1px;
    background: #eee;
    margin: 1rem 0;
  }

  .benefit-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 6px 0;
    label {
      font-size: $text-sm;
      display: flex;
      align-items: center;
      gap: 6px;
      color: #444;
    }
    .muted {
      color: #d72638;
      font-weight: 600;
      font-size: $text-sm;
    }
  }

  .total-row {
    border-top: 1px solid #eee;
    margin-top: 1rem;
    padding-top: 0.8rem;
    display: flex;
    justify-content: space-between;
    font-weight: 700;
    color: #222;
    strong {
      color: #3e9c9b;
   
    }
  }
}

/* ✅ 결제 카드 */
.payment_card {
  button{
    font-size: $text-md;
color: #444444;
  }
  .card_header {
    margin-bottom: 1rem;
  }
  h3 {
    font-size: $text-md;
    font-weight: 700;
    color: #222;
  }

  .pay-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 10px;

    @media (max-width: 1024px) {
      grid-template-columns: repeat(2, 1fr);
      gap: 14px;
    }

    @media (max-width: 600px) {
      grid-template-columns: 1fr;
    }
  }

  .pay-card {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    border: 1px solid #ddd;
    border-radius: 8px;
    background: #fff;
    cursor: pointer;
    padding: 18px 0;
    transition: 0.3s;

    &:hover {
      border-color: $color_main ;
      background: #f5fcfc;
    }

    &.selected {
      border-color: $color_main ;
      background: #e8f9f8;
      color: $color_main ;
      font-weight: 600;
    }

    .pay-img {
      width: 60%;
      max-width: 110px;
      height: auto;
      object-fit: contain;

      @media (max-width: 600px) {
        width: 55%;
      }
    }

    span {
      margin-top: 8px;
      font-size: $text-sm;
    }
@media (max-width: 1024px) {
    flex-direction: row;
  }
@media (max-width: 600px) {
   gap: 3%
  }

  }
}

/* ✅ 결제 버튼 */
.submit_btn {
  width: 50%;
  padding: 14px 0;
  font-weight: 700;
  font-size: $button;
  color: #fff;
  background: $color_main ;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  margin: 20px auto 0;
  display: block;
  transition: 0.3s;

  &:hover {
    background: #3A8C88;
  }

  @media (max-width: 1024px) {
    width: 75%;
  }

  @media (max-width: 600px) {
    width: 100%;
  }
}

// ===========추가========
</style>
