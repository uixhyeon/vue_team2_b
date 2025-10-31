<template>
  <!-- 결제완료 -->
  <section class="reserve-page">
    <div class="inner">
      <Stepper :current-step="3" />
      <div class="card-test">
        <div class="form_card receipt_card">
          <div class="card_header">
            <h3>결제 완료</h3>
          </div>

          <div class="card_content">
            <table class="receipt_table">
                <colgroup>
    <col style="width: 65%;" />   <!-- 왼쪽 -->
    <col style="width: 35%;" />   <!-- 오른쪽 -->
  </colgroup>
       <tbody>
                <tr><td>결제 수단</td><td>{{ paymentLabel }}</td></tr>
                <tr v-if="useCoupon"><td>쿠폰 할인</td><td>- {{ formatKrw(3000) }}</td></tr>
                <tr v-if="usePoints"><td>포인트 사용</td><td>- {{ formatKrw(2500) }}</td></tr>
                <tr class="total">
                  <td>최종 결제 금액</td>
                  <td><strong>{{ formatKrw(finalTotal) }}</strong></td>
                </tr>
              </tbody>
            </table>

            <div class="receipt_footer">
              <p>주문번호 : 2025-1023-001</p>
            <p>결제일시 : {{ formattedNow }}</p>
            </div>
          </div>

          <div class="card_header">
            <h3>예약 완료</h3>
          </div>

          <div class="card_content">
            <table class="receipt_table">
                       <colgroup>
    <col style="width: 65%;" />   <!-- 왼쪽 -->
    <col style="width: 35%;" />   <!-- 오른쪽 -->
  </colgroup>
        <tbody>
                <!-- 사물함 예약 -->
                <tr v-if="form.name"><td>성함</td><td>{{ form.name }}</td></tr>
                <tr v-if="form.phone"><td>휴대폰</td><td>{{ form.phone }}</td></tr>
                <tr v-if="form.size"><td>사물함 사이즈</td><td>{{ form.size }}</td></tr>
                <tr v-if="form.address"><td>대여 장소</td><td>{{ form.address }}</td></tr>
                <tr v-if="form.dateRange && form.dateRange[0] && form.dateRange[1]">
                  <td>예약 기간</td>
                  <td>{{ form.dateRange[0] }} ~ {{ form.dateRange[1] }}</td>
                </tr>

                <!-- 짐 가져오기 -->
                <tr v-if="form.pickupAddress"><td>픽업 주소</td><td>{{ form.pickupAddress }}</td></tr>
                <tr v-if="form.pickupAddressDetail"><td>상세 주소</td><td>{{ form.pickupAddressDetail }}</td></tr>
                <tr v-if="form.pickupDate"><td>픽업일</td><td>{{ form.pickupDate }}</td></tr>

                <!-- 집으로 보내기 -->
                <tr v-if="form.homeAddress"><td>배송 주소</td><td>{{ form.homeAddress }}</td></tr>
                <tr v-if="form.homeAddressDetail"><td>상세 주소</td><td>{{ form.homeAddressDetail }}</td></tr>
                <tr v-if="form.deliveryDate"><td>배송일</td><td>{{ form.deliveryDate }}</td></tr>
              </tbody>
            </table>
          </div>
        </div>

        <button class="submit_btn" @click="goToHome">홈으로 이동</button>
      </div>
    </div>
  </section>
</template>
<script setup>
import { useRoute, useRouter } from "vue-router";
import { ref, computed } from "vue";
import Stepper from "@/components/reserv/Stepper.vue";

const route = useRoute();
const router = useRouter();

// ✅ 전달된 데이터 받기
const form = ref(
  route.query.form
    ? JSON.parse(route.query.form)
    : {
        name: "",
        phone: "",
        size: "",
        address: "",
        dateRange: [],
        pickupAddress: "",
        pickupAddressDetail: "",
        pickupDate: "",
        homeAddress: "",
        homeAddressDetail: "",
        deliveryDate: "",
      }
);

const useCoupon = ref(route.query.useCoupon === "true");
const usePoints = ref(route.query.usePoints === "true");
const selectedPayment = ref(route.query.payment || "card");
const total = Number(route.query.total) || 0;

// 결제 수단명
const paymentLabel = computed(() => {
  switch (selectedPayment.value) {
    case "card": return "💳 신용카드";
    case "kakao": return "💬 카카오페이";
    case "naver": return "N Pay";
    case "bank": return "🏦 무통장입금";
    default: return "-";
  }
});

// 할인 계산
const discount = computed(() => {
  let d = 0;
  if (useCoupon.value) d += 3000;
  if (usePoints.value) d += 2500;
  return d;
});

const finalTotal = computed(() => total);

// 통화 포맷
const formatKrw = (v) =>
  new Intl.NumberFormat("ko-KR", { style: "currency", currency: "KRW" }).format(v);

// 현재 시각
const formattedNow = new Date().toLocaleString("ko-KR", {
  year: "numeric",
  month: "2-digit",
  day: "2-digit",
  hour: "2-digit",
  minute: "2-digit",
});

// 홈으로 이동
const goToHome = () => {
  router.push("/");
};
</script>

<style scoped lang="scss">
@use "/src/assets/style/variables" as *;

//====배경 레아웃================
.reserve-page {
  background: #f5f7f7;
  // padding: 5rem 0;
  // min-height: 60rem;
}

.inner {
  width: 100%;
  max-width: 1120px; 
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  align-items: center; /* Stepper + 카드 중앙 정렬 */
 padding: 40px 0 80px 0;
}

//====공통================
.form_card {
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.06); 
  border: 1px solid #fff; 
  position: relative;
  width: 100%;           
  max-width: 600px;      
  padding: 30px 5vw;    
  box-sizing: border-box; 

  /* 상단 색상바 (브랜드 컬러) */
  &::before {
    content: "";
    position: absolute;
    top: 0; left: 0;
    width: 100%; 
    height: 12px;
    background: $color_main;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }
  .card_header h3 {
    font-size:$text-md;
    font-weight: 600;
    color: #333;
    margin: 0 0 15px;
  }
  @media (max-width: 768px) {
  .form_card {
    max-width: 90%;      
    padding: 24px 20px;  
  }

  .receipt_table td {
    font-size: $label-sm; 
  }

  .submit_btn {
    width: 100%;         
    max-width: none;
  }
}
}

// 결제완료
.receipt_card {
  text-align: center;

  .check_icon {
    font-size:$title-md;
    margin-bottom: 10px;
  }

  h4 {
    font-size: 18px;
    color: #222;
    margin-bottom: 20px;
  }

//표
  .receipt_table {
    width: 100%;
    font-size: $text-sm;
    border-collapse: collapse;
    margin-bottom: 24px;

    tr {
      border-bottom: 1px solid #e7e7e7;

      td {
        padding: 10px 0;
        text-align: left;
        color: #444;

        &:first-child {
          width: 45%;
          color: #777;
        }
      }

      &.total td {
        font-weight: 500;
        color: #111;

        &:last-child {
          color: $color_main;
          font-size:$text-md;
        }
      }
    }
  }

  // 하단 문구 
  .receipt_footer {
    text-align: left;
    font-size: $label-md;
    color: #888;
    border-top: 1px dashed #e7e7e7;
    padding-top: 10px;
    margin-bottom: 20px;
  }
}

//=======버튼=============
.submit_btn {
  width: 80%;
  margin-top: 20px;
  max-width: 300px;
  padding: 14px 0;
  font-size: $button;
  font-weight: 600;
  color: #fff;
  background: $color_main;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: background 0.2s ease; 

  &:hover {
    background: $color_main_deep;
  }
}

//====================
.card-test {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  background: #f5f7f7;
}

</style>
