<template>
  <div class="form_card" :class="{ open: isOpen }">
    <!-- ✅ 카드 헤더 클릭으로 열기/닫기 -->
    <div class="card_header" @click="$emit('toggle')">
      <h3>사물함 예약*</h3>

      <!-- ✅ 모든 입력 완료 시 체크 아이콘 표시 -->
      <i
        v-if="isComplete"
        class="fa-solid fa-check"
        style="color:#4CAF50; font-size:1.1rem;"
      ></i>
    </div>

    <!-- ✅ 내용 (토글로 열고닫기) -->
    <transition name="fade">
      <div v-show="isOpen" class="card_content" @click.stop>
        <!-- 이름 -->
       <div class="form_group">
  <label>성함*</label>
  <input
    type="text"
    placeholder="성함을 입력해주세요"
    v-model="localForm.name"
     @focus="$emit('touch', 'name')"
  />
  <p v-if="touched.name && errors.name" class="error">{{ errors.name }}</p>
</div>

<div class="form_group">
  <label>휴대폰 번호*</label>
  <input
    type="text"
    placeholder="01012345678"
    v-model="localForm.phone"
      @focus="$emit('touch', 'phone')"
  />
 
  <p v-if="touched.phone && errors.phone" class="error">{{ errors.phone }}</p>
</div>

<div class="form_group">
  <label>사물함 사이즈*</label>
  <select v-model="localForm.size"
   @focus="$emit('touch', 'size')"
  >
    <option value="">사이즈를 선택해 주세요</option>
    <option>S</option>
    <option>M</option>
    <option>L</option>
    <option>XL</option>
    <option>XXL</option>
  </select>

  <p v-if="touched.size && errors.size" class="error">{{ errors.size }}</p>
</div>

<div class="form_group">
  <label>대여 장소*</label>
  <div class="addr-input">
    <input
      type="text"
      placeholder="지점 선택"
      v-model="localForm.address"
      readonly
        @focus="$emit('touch', 'address')"
    />
    <button type="button" class="mini-btn" @click="$emit('openBranch')">
      지점 선택
    </button>
  </div>
 
  <p v-if="touched.address && errors.address" class="error">{{ errors.address }}</p>
</div>

<div class="form_group">
  <label>예약 기간*</label>
  <VueDatePicker
    v-model="localForm.dateRange"
    range
    locale="ko"
    :enable-time-picker="false"
    format="yyyy-MM-dd"
    placeholder="기간을 선택하세요"
  />

  <p v-if="touched.dateRange && errors.dateRange" class="error">{{ errors.dateRange }}</p>
</div>

      </div>
    </transition>
  </div>
</template>



<script setup>
import { computed } from "vue";
import VueDatePicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";

const props = defineProps({
  form: { type: Object, required: true },
  isOpen: { type: Boolean, default: true },
  errors: { type: Object, default: () => ({}) },
   touched: { type: Object, default: () => ({}) }, 
});
const emit = defineEmits(["update:form", "openBranch", "toggle", "touch"]);

const localForm = computed({
  get: () => props.form,
  set: (val) => emit("update:form", val),
});

// ✅ 모든 필수항목이 채워졌을 때 체크 아이콘 표시
const isComplete = computed(() => {
  const f = props.form;
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
</script>

<style scoped lang="scss">
@use "/src/assets/style/variables" as *;
/* =========================================================
 ✅ width:100%인데 화면에 꽉 차지 않는 이유
========================================================= */
/*
  ⚙️ 핵심 요약:
  .form_card의 width:100%는 "직계 부모의 콘텐츠 영역 안에서" 100%를 의미함.
  즉, 부모가 폭을 제한하고 있으면 자식은 그 한도 내에서만 100%를 차지함.

  현재 구조는 다음과 같음:

  <div class="background inner">
    <div class="container">
      <div class="left">
        <div class="form_card">...</div>
      </div>
    </div>
  </div>

  🔹 폭이 줄어드는 이유:
  1) .inner  → max-width: 1120px + padding: 0 40px;
      → 전체 페이지 폭이 제한되고 좌우 여백이 생김.
  2) .container → display:grid; grid-template-columns: 3fr 2fr;
      → 왼쪽 영역(3fr)과 오른쪽 영역(2fr)으로 나뉘어 있음.
  3) .left → flex-direction: column; gap: 1.3rem;
      → 카드들 사이 간격을 띄워줌.
  4) .form_card → width:100%;
      → 부모(.left)의 폭을 전부 채우지만, 그 부모 자체가 이미 좁음.

  따라서 “화면 전체 폭”이 아니라 “왼쪽 열(3fr)” 영역만 채워짐.

  🧭 해결 방법 요약:
  - 전체 폭으로 꽉 채우고 싶다면:
      .inner, .container의 max-width, padding을 해제해야 함.
      (예: max-width:none; padding:0;)
  - 현재처럼 그리드 구조(왼쪽 카드 + 오른쪽 요약) 유지 시:
      .form_card는 width:100% 상태 그대로 두면 됨.
*/
/* =========================================================
 ✅ FORM CARD (입력 카드)
========================================================= */
.form_card {
  background: #fff;
  border-radius: 10px;
  border: 1px solid #f0f0f0;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  position: relative;
  padding: 30px 40px;
  transition: all 0.25s ease;
  color: #444;
  font-size: $text-sm;
  box-sizing: border-box;

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

  &.open {
    box-shadow: 0 6px 16px rgba(0, 0, 0, 0.08);
    // transform: translateY(-2px);
  }

  /* 헤더 (제목 + 체크아이콘) */
  .card_header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;
    margin-bottom: 15px;

    h3 {
      font-size: $text-md;
      font-weight: 600;
      color: #333;
      margin: 0;
    }

    i {
      font-size: 1rem;
      color: $color_main;
    }
  }

  /* 폼 그룹 */
  .form_group {
    margin-bottom: 20px;

    label {
      display: block;
      font-size: $label-md;
      font-weight: 500;
      color: #555;
      margin-bottom: 6px;
      padding-left: 3px;
    }

    input,
    select {
      width: 100%;
      border: none;
      border-bottom: 1px solid #e7e7e7;
      background: transparent;
      font-size: $label-md;
      color: #333;
      padding: 10px;
      transition: border-color 0.2s ease;

      &:focus {
        border-bottom: 1px solid $color_main_light;
        outline: none;
      }

      &::placeholder {
        color: #aaa;
      }
    }

    .label {
      font-size: $label-sm;
      color: #888;
      margin-top: 4px;
      padding-left: 3px;
    }
  }

  /* 주소 입력 행 */
  .addr-input {
    display: flex;
    gap: 8px;
    align-items: center;

    .mini-btn {
      padding: 8px 12px;
      border-radius: 6px;
      background: $color_main;
      color: #fff;
      border: none;
      cursor: pointer;
      font-size: $label-sm;
      transition: 0.2s;

      &:hover {
        background: $color_main_deep;
      }
    }
  }

  // /* 반응형 */
  // @media (max-width: 1024px) {
  //   padding: 25px 30px;
  // }

  // @media (max-width: 480px) {
  //   padding: 20px;
  //   .card_header h3 {
  //     font-size: $label-md;
  //   }
  // }
}
// 열닫추가
.form_card {
  transition: box-shadow 0.25s ease, transform 0.25s ease;
}


.error {
  color: #e53935;
  font-size: 0.85rem;
  margin-top: 4px;
  padding-left: 3px;
  line-height: 1.4;
}

/* =========================================================
 ✅ VueDatePicker (날짜 선택기) 통일 커스터마이징
========================================================= */

/* 📌 기본 입력 필드 통일 */
.form_group {
  .dp__main,
  .dp__input_wrap {
    width: 100%;
    background: transparent !important;
    border: none !important;
    box-shadow: none !important;
  }

  /* 📌 내부 인풋 (placeholder, focus 효과 포함) */
  :deep(.dp__input) {
    width: 100%;
    background: transparent !important;
    border: none !important;
    border-bottom: 1px solid #e7e7e7 !important;
    border-radius: 0 !important;
    box-shadow: none !important;
    padding: 10px 10px !important;
    font-size: $label-md !important;
    color: #333 !important;
    transition: border-color 0.25s ease;

    &::placeholder {
      color: #777 !important;
    }

    &:focus {
      border-bottom: 1px solid $color_main_light !important;
      outline: none !important;
    }
  }

  /* 📌 포커스 시 테두리 컬러 유지 */
  :deep(.dp__input:focus) {
    border-bottom: 1px solid $color_main_light !important;
    outline: none !important;
  }

  /* 📌 기본 달력 아이콘 제거 */
  :deep(.dp__input_icon) {
    display: none !important;
  }

  /* 📌 팝업 달력의 외곽 메뉴 스타일 */
  :deep(.dp__outer_menu_wrap) {
    border-radius: 10px !important;
    border: 1px solid #ddd !important;
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1) !important;
  }
}

/* =========================================================
 ✅ VueDatePicker - 브랜드 컬러 테마 (라이트/다크 공통)
========================================================= */
:deep(.dp__theme_light),
:deep(.dp__theme_dark) {
  --dp-primary-color: #53b4a1 !important; /* ✅ 선택된 날짜 배경 */
  --dp-primary-text-color: #fff !important; /* 선택된 날짜 텍스트 */
  --dp-hover-color: #449b8a !important; /* hover 배경 */
  --dp-hover-text-color: #fff !important;
  --dp-range-between-dates-background-color: #eaf8f6 !important; /* 기간 중간색 */
  --dp-border-color: #d2e8e8 !important;
  --dp-menu-border-color: #d2e8e8 !important;
  --dp-success-color: #53b4a1 !important; /* Select 버튼 색 */
  --dp-icon-color: #53b4a1 !important; /* 내부 달력 화살표 */
  --dp-text-color: #333 !important;
  --dp-hover-icon-color: #3a8c88 !important;
  --dp-secondary-color: #f7fcfb !important;
}

/* =========================================================
 ✅ 반응형 처리
========================================================= */
@media (max-width: 768px) {
  :deep(.dp__outer_menu_wrap) {
    width: 95vw !important;
  }
  :deep(.dp__calendar_header) {
    font-size: 0.85rem !important;
  }
  :deep(.dp__calendar_item) {
    font-size: 0.8rem !important;
  }
}

</style>
