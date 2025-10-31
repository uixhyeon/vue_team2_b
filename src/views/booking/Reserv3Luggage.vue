<template>
  <div class="form_card" :class="{ open: isOpen }">
    <!-- ✅ 헤더 클릭으로 토글 -->
    <div class="card_header" @click="$emit('toggle')">
      <h3>집으로 보내기</h3>

      <!-- ✅ 모든 입력 완료 시 체크 아이콘 표시 -->
      <i
        v-if="isComplete"
        class="fa-solid fa-check"
        style="color:#4CAF50; font-size:1.1rem;"
      ></i>
    </div>

    <!-- ✅ 내용 -->
    <transition name="fade">
      <div v-show="isOpen" class="card_content" @click.stop>
        <!-- 주소 입력 -->
        <div class="form_group">
          <label>주소*</label>
          <div class="addr-input">
            <input
              type="text"
              placeholder="받으실 주소를 선택해주세요"
              v-model="localForm.homeAddress"
              readonly
              @focus="$emit('touch', 'homeAddress')"
            />
            <button type="button" class="mini-btn" @click="$emit('openHome')">
              주소 검색
            </button>
          </div>
          <p v-if="touched.homeAddress && errors.homeAddress" class="error">
            {{ errors.homeAddress }}
          </p>
        </div>

        <!-- 상세주소 -->
        <div class="form_group">
          <label>상세주소*</label>
          <input
            type="text"
            placeholder="상세주소를 입력해주세요"
            v-model="localForm.homeAddressDetail"
            @focus="$emit('touch', 'homeAddressDetail')"
          />
          <p
            v-if="touched.homeAddressDetail && errors.homeAddressDetail"
            class="error"
          >
            {{ errors.homeAddressDetail }}
          </p>
        </div>

        <!-- 배송일 -->
        <div class="form_group">
          <label>배송일*</label>
          <VueDatePicker
            v-model="localForm.deliveryDate"
            locale="ko"
            :enable-time-picker="false"
            format="yyyy-MM-dd"
            placeholder="날짜를 선택하세요"
            @focus="$emit('touch', 'deliveryDate')"
          />
          <p v-if="touched.deliveryDate && errors.deliveryDate" class="error">
            {{ errors.deliveryDate }}
          </p>
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
  touched: { type: Object, default: () => ({}) }, // ✅ 추가
});

const emit = defineEmits(["update:form", "openHome", "toggle", "touch"]);

const localForm = computed({
  get: () => props.form,
  set: (val) => emit("update:form", val),
});

// ✅ 모든 입력 완료 시 체크 아이콘 표시
const isComplete = computed(() => {
  const f = props.form;
  return f.homeAddress && f.homeAddressDetail && f.deliveryDate;
});
</script>

<style scoped lang="scss">
@use "/src/assets/style/variables" as *;

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
  }

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
  }

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
}

/* 에러 문구 스타일 */
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
