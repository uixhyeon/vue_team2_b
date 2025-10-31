<template>
  <div class="stepper">
    <ol>
      <li
        v-for="(step, i) in steps"
        :key="i"
        class="step"
        :class="{ active: currentStep === i + 1 }"
        v-show="windowWidth > 768 || currentStep === i + 1"
      >
        <div class="circle"></div>
        <p>{{ step }}</p>
        <div v-if="i < steps.length - 1 && windowWidth > 768" class="dotline"></div>
      </li>
    </ol>
  </div>
</template>

<script setup>
import { ref, onMounted, onUnmounted } from "vue";

defineProps({
  currentStep: {
    type: Number,
    default: 1, // ✅ 기본값: 첫 번째 단계 활성화
  },
});

const steps = ["예약 하기", "확인 및 결제", "변경 완료"];

/* ✅ 반응형 감지 */
const windowWidth = ref(window.innerWidth);
const handleResize = () => (windowWidth.value = window.innerWidth);

onMounted(() => window.addEventListener("resize", handleResize));
onUnmounted(() => window.removeEventListener("resize", handleResize));
</script>

<style lang="scss" scoped>
@use "/src/assets/style/variables" as *;
// body{
//   // background-color: #f5f7f7;
// }

/* ✅ 상단 스텝 표시 */
.stepper {
  // background-color: #f5f7f7;
  display: flex;
  justify-content: center;
  align-items: center;
  margin: 3rem auto;

  ol {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: clamp(2rem, 5vw, 5rem);
    list-style: none;
    padding: 0;
    margin: 0;
  }
}

/* ✅ 각 단계 (step) */
.step {
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  text-align: center;
  font-size: 18px;
  color: #bbb;
  font-weight: 400;

  /* ✅ 원(circle) */
  .circle {
    position: relative;
    width: 22px;
    height: 22px;
    border-radius: 50%;
    border: 3px solid #bbb;
    background-color: #e6e6e6;
    margin-bottom: 0.6rem;
    z-index: 2;

    /* ✅ 점선 중앙 정렬 */
&::after {
  content: "";
  position: absolute;
  top: calc(50% - 1px);
  left: 2rem;
  transform: translateY(-50%);
  width: calc(clamp(2rem, 5vw, 5rem) +  20px); // ✅ +8px 정도 여유
  border-top: 1px solid #bbb;
  z-index: 1;
}
  }

  &:last-child .circle::after {
    display: none;
  }

  /* ✅ 활성화 상태 */
  &.active {
    color:$color_main ;
    font-weight: 600;

    .circle {
      border-color: $color_main ;
      background-color:$color_main_background;

      &::after {
        border-top-color: $color_main;
      }
    }
  }
}

// 반응형
@media (max-width: 768px) {
  .stepper ol {
    flex-direction: column;
    align-items: center;
  }

  /* 기본적으로 모든 스텝 숨김 */
  .step {
    display: none;
  }

  /* 현재 활성 스텝만 표시 */
  .step.active {
    display: flex;
    flex-direction: column;
    align-items: center;
  }

  /* 점선 완전 제거 */
  .dotline,
  .circle::after {
    display: none !important;
  }
}
</style>
