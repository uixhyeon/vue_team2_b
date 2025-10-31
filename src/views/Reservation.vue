<template>
  <div class="wrap">
    <Stepper :current-step="1" />

    <!-- ✅ 통일된 배경/레이아웃 -->
    <div class="background inner">
      <div class="container">
        <!-- 왼쪽 입력 카드들 -->
        <div class="left">
        <Reserv1Locker
  v-model:form="form"
  :isOpen="openSection === 'locker'"
  :errors="errors"
  :touched="touched"
  @toggle="toggleSection('locker')"
  @openBranch="showBranchModal = true"
  @touch="handleTouch"
/>


       <Reserv2Arrival
  v-model:form="form"
  :isOpen="openSection === 'arrival'"
  :errors="errors"
  :touched="touched"
  @toggle="toggleSection('arrival')"
  @openPickup="openPickupAddr = true"
  @touch="handleTouch"
/>


          <Reserv3Luggage
            v-model:form="form"
            :isOpen="openSection === 'luggage'"
            :errors="errors"
            :luggage-complete="luggageComplete"
            @toggle="toggleSection('luggage')"
            @openHome="openHomeAddr = true"
          />
        </div>

        <!-- 오른쪽 요약 카드 -->
        <div class="right">
          <Reserv4Summary
            :form="form"
            :totalPrice="totalPrice"
            :hasInput="hasInput"
            :lockerComplete="lockerComplete"
            :arrivalComplete="arrivalComplete"
            :luggageComplete="luggageComplete"
            :errors="errors"
            @submit="handleSubmit"
          />

          <!-- ✅ 입력 완료 버튼 -->
          <button class="submit_btn" @click="handleSubmit">입력 완료</button>
        </div>
      </div>
    </div>

    <!-- ===== 모달들 ===== -->
    <BranchSelectModal
      :open="showBranchModal"
      :locations="locations"
      @close="showBranchModal = false"
      @selected="handleBranchSelect"
    />

    <AddressPicker
      v-model="form.pickupAddress"
      :open="openPickupAddr"
      @close="openPickupAddr = false"
      @selected="(addr) => (form.pickupAddress = addr)"
    />

    <AddressPicker
      v-model="form.homeAddress"
      :open="openHomeAddr"
      @close="openHomeAddr = false"
      @selected="(addr) => (form.homeAddress = addr)"
    />
  </div>
</template>


<script setup>
import { ref, computed, watch } from "vue";
import { useRouter, useRoute } from "vue-router";

//const router = useRouter(); // ✅ 반드시 선언



import Reserv1Locker from "@/views/booking/Reserv1Locker.vue";
import Reserv2Arrival from "@/views/booking/Reserv2Arrival.vue";
import Reserv3Luggage from "@/views/booking/Reserv3Luggage.vue";
import Reserv4Summary from "@/views/booking/Reserv4Summary.vue";
import BranchSelectModal from "@/components/reserv/BranchSelectModal.vue";
import AddressPicker from "@/components/reserv/AddressPicker.vue";



// 공통 폼 상태
const form = ref({
  name: "",
  phone: "",
  size: "",
  address: "",
  dateRange: null,
  pickupAddress: "",
  pickupAddressDetail: "",
  pickupDate: "",
  homeAddress: "",
  homeAddressDetail: "",
  deliveryDate: "",
});

// 사이즈별 요금표
const prices = {
  S: { locker: 5000, delivery: 4000 },
  M: { locker: 8000, delivery: 6000 },
  L: { locker: 15000, delivery: 14000 },
  XL: { locker: 20000, delivery: 22000 },
  XXL: { locker: 28000, delivery: 32000 },
};

// 완료 상태
const lockerComplete = computed(() => {
  const f = form.value;
  return (
    f.name &&
    f.phone &&
    f.size &&
    f.address &&
    f.dateRange &&
    f.dateRange[0] &&
    f.dateRange[1]
  );
});


// ====== 섹션 열기/닫기 중앙관리 ======
const openSection = ref("locker");
const toggleSection = (name) => {
  const f = form.value;

  // ✅ 1. 사물함 예약 완료 전에는 다른 카드 접근 불가
  if (!lockerComplete.value && name !== "locker") {
    alert("먼저 사물함 예약을 완료해주세요.");
    return;
  }

  // ✅ 2. 현재 열려 있던 카드명 기억
  const prevSection = openSection.value;

  // ✅ 3. 전환 전 미완성 카드 자동 초기화
  if (prevSection === "arrival") {
    const filled =
      f.pickupAddress?.trim() &&
      f.pickupAddressDetail?.trim() &&
      f.pickupDate;

    if (!filled) {
      f.pickupAddress = "";
      f.pickupAddressDetail = "";
      f.pickupDate = "";
    }
  }

  if (prevSection === "luggage") {
    const filled =
      f.homeAddress?.trim() &&
      f.homeAddressDetail?.trim() &&
      f.deliveryDate;

    if (!filled) {
      f.homeAddress = "";
      f.homeAddressDetail = "";
      f.deliveryDate = "";
    }
  }

  // ✅ 4. 토글 (닫기 or 새 카드 열기)
  openSection.value = openSection.value === name ? null : name;

  // ✅ 5. 반응성 강제 갱신 (Vue가 내부 변경 감지하도록)
  form.value = { ...f };
};





const arrivalComplete = computed(() => {
  const f = form.value;
  return f.pickupAddress && f.pickupAddressDetail && f.pickupDate;
});

const luggageComplete = computed(() => {
  const f = form.value;
  return f.homeAddress && f.homeAddressDetail && f.deliveryDate;
});


// ====== 에러 상태 ======
const errors = ref({});

// ====== 유효성 검사 ======
const validateForm = () => {
  const f = form.value;
  const err = {};

  // 1) 사물함 예약 필수
  if (!f.name || !f.name.trim()) err.name = "이름을 입력해주세요";
  if (!f.phone || !/^(010|011|016|017|018|019)\d{7,8}$/.test(f.phone))
    err.phone = "휴대폰 번호 형식이 올바르지 않습니다";
  if (!f.size) err.size = "사물함 사이즈를 선택해주세요";
  if (!f.address) err.address = "대여 장소를 선택해주세요";
  if (!f.dateRange || f.dateRange.length < 2)
    err.dateRange = "예약 기간을 선택하고 확인을 눌러주세요";

  // 2) 짐 가져오기(선택인데 입력했으면 제대로)
  if (f.pickupAddress || f.pickupAddressDetail || f.pickupDate) {
    if (!f.pickupAddress) err.pickupAddress = "픽업 주소를 선택해주세요";
    if (!f.pickupAddressDetail) err.pickupAddressDetail = "픽업 상세주소를 입력해주세요";
    if (!f.pickupDate) err.pickupDate = "픽업일을 선택하고 확인을 눌러주세요";
  }

  // 3) 집으로 보내기(선택인데 입력했으면 제대로)
  if (f.homeAddress || f.homeAddressDetail || f.deliveryDate) {
    if (!f.homeAddress) err.homeAddress = "배송 주소를 선택해주세요";
    if (!f.homeAddressDetail) err.homeAddressDetail = "배송 상세주소를 입력해주세요";
    if (!f.deliveryDate) err.deliveryDate = "배송일을 선택하고 확인을 눌러주세요";
  }

  errors.value = err;
  return Object.keys(err).length === 0;
};
// ✅ 포커스 시 해당 필드를 'touched'로 변경
function handleTouch(field) {
  touched.value[field] = true;
}
// =============여긴가====
const touched = ref({
  name: false,
  phone: false,
  size: false,
  address: false,
  dateRange: false,
  pickupAddress: false,
  pickupAddressDetail: false,
  pickupDate: false,
  homeAddress: false,
  homeAddressDetail: false,
  deliveryDate: false,
});

// ✅ 실시간 감시로 에러 즉시 업데이트
// 이미 맨 위에서 import 안 되어 있으면 추가

watch(
  form,
  (f) => {
    const err = {};

    // 🧩 사물함 예약 실시간 검증
    if (!f.name?.trim()) err.name = "이름을 입력해주세요";
    if (!f.phone || !/^(010|011|016|017|018|019)\d{7,8}$/.test(f.phone))
      err.phone = "휴대폰 번호 형식이 올바르지 않습니다";
    if (!f.size) err.size = "사물함 사이즈를 선택해주세요";
    if (!f.address) err.address = "대여 장소를 선택해주세요";
    if (!f.dateRange || f.dateRange.length < 2)
      err.dateRange = "예약 기간을 선택해주세요";

    // 🧩 짐 가져오기 실시간 검증
    if (f.pickupAddress || f.pickupAddressDetail || f.pickupDate) {
      if (!f.pickupAddress) err.pickupAddress = "픽업 주소를 선택해주세요";
      if (!f.pickupAddressDetail)
        err.pickupAddressDetail = "픽업 상세주소를 입력해주세요";
      if (!f.pickupDate)
        err.pickupDate = "픽업일을 선택하고 확인을 눌러주세요";
    }

    // 🧩 집으로 보내기 실시간 검증
    if (f.homeAddress || f.homeAddressDetail || f.deliveryDate) {
      if (!f.homeAddress) err.homeAddress = "배송 주소를 선택해주세요";
      if (!f.homeAddressDetail)
        err.homeAddressDetail = "배송 상세주소를 입력해주세요";
      if (!f.deliveryDate)
        err.deliveryDate = "배송일을 선택하고 확인을 눌러주세요";
    }

    errors.value = err;
  },
  { deep: true } // ✅ form 내부 모든 필드 감시
);



// 입력 감지 (요약 안내문용)
const hasInput = computed(() => {
  const f = form.value;
  return (
    f.name ||
    f.phone ||
    f.size ||
    f.address ||
    f.dateRange ||
    f.pickupAddress ||
    f.homeAddress
  );
});


// ===== 모달 열림 상태 =====
const showBranchModal = ref(false); // 지점 선택 (Reserv1Locker용)
const openPickupAddr = ref(false);  // 짐 가져오기 주소
const openHomeAddr = ref(false);    // 집으로 보내기 주소



// ===== 지점 선택 핸들러 =====
function handleBranchSelect(location) {
  form.value.address = location.name; // BranchSelectModal에서 전달한 값
  showBranchModal.value = false;
}
// 📍 지역별 지점 리스트
const locations = [
  {
    region: "부산 광안리",
    branches: [
      {
        id: 1,
        name: "광안리 해변점",
        address: "부산광역시 수영구 광안해변로 203",
        lockers: "S~XL 보유",
        status: "운영중",
      },
      {
        id: 2,
        name: "광안시장점",
        address: "부산광역시 수영구 남천동로 12-1", // 📍 실제 존재 주소
        lockers: "S~L 보유",
        status: "운영중",
      },
      {
        id: 3,
        name: "광안역점",
        address: "부산광역시 수영구 광안로 45",
        lockers: "M~XXL 보유",
        status: "점검중",
      },
    ],
  },
  {
    region: "강릉시",
    branches: [
      {
        id: 4,
        name: "강릉역점",
        address: "강원특별자치도 강릉시 용지로 123", // 📍 '강원특별자치도'로 변경됨
        lockers: "S~L 보유",
        status: "운영중",
      },
      {
        id: 5,
        name: "경포해변점",
        address: "강원특별자치도 강릉시 창해로 240-3", // 📍 경포해수욕장 인근 실제 도로명
        lockers: "S~XL 보유",
        status: "운영중",
      },
    ],
  },
  {
    region: "속초",
    branches: [
      {
        id: 6,
        name: "속초중앙시장점",
        address: "강원특별자치도 속초시 중앙로 147", // 📍 중앙시장 중심 좌표
        lockers: "S~L 보유",
        status: "운영중",
      },
      {
        id: 7,
        name: "속초해수욕장점",
        address: "강원특별자치도 속초시 해오름로 190", // 📍 실제 해수욕장 중심 위치
        lockers: "M~XL 보유",
        status: "점검중",
      },
    ],
  },
  {
    region: "전주",
    branches: [
      {
        id: 8,
        name: "전주한옥마을점",
        address: "전라북도 전주시 완산구 기린대로 99", // 📍 실제 한옥마을 입구 인근
        lockers: "S~XL 보유",
        status: "운영중",
      },
    ],
  },
  {
    region: "제주도",
    branches: [
      {
        id: 9,
        name: "제주시청점",
        address: "제주특별자치도 제주시 관덕로 9", // 📍 제주시청 맞은편
        lockers: "S~XL 보유",
        status: "운영중",
      },
      {
        id: 10,
        name: "서귀포점",
        address: "제주특별자치도 서귀포시 중문관광로 72", // 📍 중문관광단지 내 위치
        lockers: "M~XXL 보유",
        status: "운영중",
      },
    ],
  },
  {
    region: "오사카",
    branches: [
      {
        id: 11,
        name: "난바역점",
        address: "Namba Station, Osaka, Japan",
        lockers: "S~L 보유",
        status: "운영중",
      },
      {
        id: 12,
        name: "우메다점",
        address: "2-14-7 Sonezaki, Kita Ward, Osaka, Japan",
        lockers: "M~XXL 보유",
        status: "운영중",
      },
    ],
  },
];



// 1️⃣ 날짜 차이 계산
const rentalDays = computed(() => {
  const range = form.value.dateRange;
  if (!range || range.length < 2) return 0;
  const start = new Date(range[0]);
  const end = new Date(range[1]);
  const diff = end - start;
  return diff > 0 ? diff / (1000 * 60 * 60 * 24) + 1 : 1; // 종료일 포함
});

// 2️⃣ 총 요금 계산 수정
const totalPrice = computed(() => {
  const f = form.value;
  const size = f.size || "";
  let total = 0;

  if (lockerComplete.value) {
    const daily = prices[size]?.locker ?? 0;
    total += daily * rentalDays.value; // ✅ 일수 반영
  }
  if (arrivalComplete.value) total += prices[size]?.delivery ?? 0;
  if (luggageComplete.value) total += prices[size]?.delivery ?? 0;

  return total;
});

// 핸드서브밋위
const router = useRouter(); // ✅ 라우터 객체 선언 (스크립트 상단쪽)

// 스크립트 제일 아랫쪽
const handleSubmit = () => {
  if (!validateForm()) {
    alert("입력값을 다시 확인해주세요.");
    // 에러난 섹션 열어주기 (이거 넣으면 UX 업)
    if (errors.value.name || errors.value.phone || errors.value.size || errors.value.address || errors.value.dateRange) {
      openSection.value = "locker";
    } else if (errors.value.pickupAddress || errors.value.pickupAddressDetail || errors.value.pickupDate) {
      openSection.value = "arrival";
    } else if (errors.value.homeAddress || errors.value.homeAddressDetail || errors.value.deliveryDate) {
      openSection.value = "luggage";
    }
    return;
  }

  // 여기서 원래 하던 라우터 이동 넣으면 됨
  // router.push({ path: "/reservation2", query: {...} })
  alert("입력 완료!");

  // ✅ 유효성 검사 통과 → Reservation2.vue로 이동
  router.push({
    path: "/reservation2",
    query: { form: JSON.stringify(form.value) }, // 폼 내용도 넘김
  });
};



</script>

<style scoped lang="scss" >
@use "/src/assets/style/variables" as *;

/* =========================================================
 ✅ 1. 전체 레이아웃 프레임
========================================================= */
.wrap {
  background: #f5f7f7;
  padding: 40px 0 100px;
  display: flex;
  flex-direction: column;
  align-items: center;
}

/* 페이지 공통 내부 여백 및 폭 */
.background.inner {
  width: 100%;
  max-width: 1120px;
  margin: 0 auto;
  padding: 0 40px;
  box-sizing: border-box;
}

/* ✅ 좌우 레이아웃 (입력/요약) */
.container {
  display: grid;
  grid-template-columns: 3fr 2fr;
  gap: 2.5rem;
  align-items: flex-start;
  width: 100%;
}

/* 왼쪽: 입력 카드 묶음 */
.left {
  display: flex;
  flex-direction: column;
  gap: 1.3rem;
}

/* 오른쪽: 요약 카드 + 버튼 */
.right {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 1.3rem;
  align-self: flex-start;
}

/* =========================================================
 ✅ 2. 카드 공통 프레임 (폭·정렬만)
========================================================= */
/*
  각 카드의 내부 내용은 자식 컴포넌트에서 이미 정의되어 있음.
  여기서는 폭(width), 외곽 여백(margin), 그림자/간격 정도만 통일.
*/

.form_card,
.summary_card {
  width: 100%;                /* 부모 column 영역 가득 */
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  border: 1px solid #f0f0f0;
  box-sizing: border-box;
  transition: all 0.25s ease;
}

/* 카드 간격 통일 */
.left .form_card + .form_card {
  margin-top: 1.3rem;
}

/* 카드 열렸을 때의 강조 */
.form_card.open {
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.08);
  transform: translateY(-1px);
}

/* =========================================================
 ✅ 3. 버튼 (요약 하단)
========================================================= */
.submit_btn {
  width: 70%;
  padding: 14px 0;
  font-weight: 600;
  font-size: $button;
  color: #fff;
  background: $color_main;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.3s ease;
  display: block;
  margin: 20px auto 0;

  &:hover {
    background: $color_main_deep;
  }
}

/* =========================================================
 ✅ 4. 반응형 (중요 구간만)
========================================================= */

/* 💻 태블릿 이하 (1열 정렬) */
@media (max-width: 1024px) {
  .container {
    grid-template-columns: 1fr;
    gap: 2rem;
    max-width: 700px;
  }

  .form_card,
  .summary_card {
    width: 100%;
  }

  .submit_btn {
    width: 90%;
  }
}

/* 📱 모바일 (좁은 여백으로 간결하게) */
@media (max-width: 480px) {
  .background.inner {
    padding: 0 20px;
  }

  .container {
    gap: 1.3rem;
  }

  .form_card,
  .summary_card {
    border-radius: 8px;
    box-shadow: 0 2px 6px rgba(0, 0, 0, 0.04);
  }

  .submit_btn {
    width: 100%;
    font-size: 0.95rem;
    padding: 12px 0;
  }
}

</style>
