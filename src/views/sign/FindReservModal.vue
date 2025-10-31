<template>
  <div class="modal-overlay" @click.self="close">
    <div class="modal-box">
      <header class="modal-header">
        <h2>예약번호 찾기</h2>
        <button class="close-btn" @click="close">&times;</button>
      </header>

      <div class="modal-body">
        <p class="desc">등록 시 사용한 이름과 휴대폰 번호를 입력해주세요.</p>
        <div class="form-group">
          <label>이름</label>
          <input type="text" v-model="name" placeholder="이름 입력" />
        </div>

        <div class="form-group">
          <label>휴대폰 번호</label>
          <input type="text" v-model="phone" placeholder="- 없이 입력" />
        </div>

        <button class="btn primary full" @click="findReservation">
          예약번호 찾기
        </button>

        <div v-if="foundCode" class="result">
          <p>예약번호: <strong>{{ foundCode }}</strong></p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
const emit = defineEmits(["close"]);

const name = ref("");
const phone = ref("");
const foundCode = ref("");

const findReservation = () => {
  if (!name.value || !phone.value) {
    alert("이름과 휴대폰 번호를 모두 입력해주세요.");
    return;
  }
  foundCode.value = "20251029-1234"; // 예시값
};

const close = () => {
  emit("close");
};
</script>

<style scoped lang="scss">
@use "/src/assets/style/variables" as *;
.modal-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.45);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 2000;
}

.modal-box {
  background: #fff;
  width: 400px;
  border-radius: 10px;
  box-shadow: 0 4px 15px rgba(0, 0, 0, 0.2);
  animation: fadeIn 0.25s ease;
  overflow: hidden;
}

.modal-header {
  background: $color_main;
  color: #fff;
  padding: 16px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.close-btn {
  background: none;
  border: none;
  font-size: 22px;
  color: #fff;
  cursor: pointer;
}

.modal-body {
  padding: 24px 28px;

  .desc {
    font-size: 14px;
    color: #555;
    margin-bottom: 18px;
  }

  .form-group {
    margin-bottom: 16px;

    label {
      font-size: 13px;
      font-weight: 600;
      display: block;
      margin-bottom: 5px;
    }

    input {
      width: 100%;
      border: 1px solid #ccc;
      border-radius: 6px;
      padding: 10px;
      outline: none;

      &:focus {
        border-color: $color_main;
      }
    }
  }

  .result {
    margin-top: 15px;
    background: #f7f7f7;
    border-radius: 6px;
    padding: 12px 15px;
    text-align: center;
  }

  .btn.primary.full {
    width: 100%;
    background: $color_main;
    color: #fff;
    padding: 12px 0;
    border-radius: 6px;
    border: none;
    cursor: pointer;
    margin-top: 8px;

    &:hover {
      background: $color_main_deep;
    }
  }
}
</style>
