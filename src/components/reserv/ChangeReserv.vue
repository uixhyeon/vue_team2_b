<template>
  <!-- ✅ 새로 추가된 프레임 -->
  <div class="page-frame">
    <div class="change-wrap">
      <!-- ✅ 상단 영역 -->
      <header class="header">
        <img src="/images/reservation/Asset2.png" alt="마타주 로고" class="logo" />
        <div class="scr">
          <h1>예약 변경</h1>
          <p class="sub">변경할 예약번호를 입력해주세요.</p>
        </div>
      </header>

      <!-- ✅ 본문 -->
      <main class="content">
        <section class="input-section">
          <label for="res-code">예약번호</label>
          <div class="input-row">
            <input
              id="res-code"
              type="text"
              placeholder="예: 20251029-1234"
              v-model="reservationCode"
            />
            <button class="btn small" @click="loadReservation">조회</button>
          </div>
        </section>

        <section v-if="reservationData" class="info-card">
          <h2>예약 정보</h2>
          <ul>
            <li><strong>이름:</strong> {{ reservationData.name }}</li>
            <li><strong>대여장소:</strong> {{ reservationData.branch }}</li>
            <li><strong>예약일:</strong> {{ reservationData.date }}</li>
            <li><strong>보관물:</strong> {{ reservationData.item }}</li>
          </ul>

          <button class="btn primary full" @click="goToEdit">예약 수정하기</button>
        </section>

        <p class="hint">
          예약번호를 모르시나요?
          <a href="#" @click.prevent="openFindModal">예약번호 찾기</a>
        </p>
      </main>

      <FindReservationModal v-if="showModal" @close="showModal = false" />
    </div>
  </div>
</template>

<script setup>
import { ref } from "vue";
import FindReservationModal from "@/views/sign/FindResarv.vue";

const reservationCode = ref("");
const reservationData = ref(null);
const showModal = ref(false);

const loadReservation = () => {
  if (!reservationCode.value) {
    alert("예약번호를 입력해주세요.");
    return;
  }

  // ✅ 실제 API 조회 대신 예시 데이터
  reservationData.value = {
    name: "정현영",
    branch: "칠성시장점",
    date: "2025-10-30",
    item: "중형 캐리어",
  };
};

const goToEdit = () => {
  alert("예약 수정 페이지로 이동합니다!");
  // router.push(`/reservation/edit/${reservationCode.value}`);
};

const openFindModal = () => {
  showModal.value = true;
};
</script>

<style scoped lang="scss">
@use "/src/assets/style/variables" as *;

/* =========================================================
   전체 구조
========================================================= */
.page-frame {
  display: flex;
  justify-content: center;
  align-items: center;
  border: 1px solid $color_main;
  border-radius: 24px;
  margin: 40px;
  background: #f6f8f8;
  padding: 40px 0;
  box-sizing: border-box;
}

/* =========================================================
   내부 컨테이너 (내용 영역)
========================================================= */
.change-wrap {
  background: #f6f8f8;
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 60px 20px;
  width: 100%;
  max-width: 640px;
}

/* =========================================================
   상단 영역 (깨짐 방지 완전 수정)
========================================================= */
.header {
  display: flex;
  align-items: center;
  justify-content: flex-start;
  flex-wrap: wrap; /* ✅ 줄바꿈 허용 */
  gap: 20px; /* ✅ 로고-텍스트 간격 */
  width: 100%;
  max-width: 640px;
  margin-bottom: 40px;
  text-align: left;

  .logo {
    width: 220px;
    max-width: 45%;
    height: auto;
    flex-shrink: 0;
  }

  .scr {
    flex: 1 1 200px;
    word-break: keep-all;
  }

  h1 {
    font-size: 26px;
    color: #222;
    font-weight: 700;
    margin-bottom: 6px;
  }

  .sub {
    font-size: 15px;
    color: #555;
  }
}

/* =========================================================
   본문
========================================================= */
.content {
  background: #fff;
  width: 100%;
  max-width: 640px;
  padding: 40px 50px;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.06);
}

.input-section {
  margin-bottom: 30px;

  label {
    font-size: 14px;
    font-weight: 600;
    color: #333;
    display: block;
    margin-bottom: 8px;
  }

  .input-row {
    display: flex;
    gap: 10px;

    input {
      flex: 1;
      border: 1px solid #ccc;
      border-radius: 6px;
      padding: 12px;
      font-size: 15px;
      outline: none;

      &:focus {
        border-color: $color_main;
      }
    }

    .btn.small {
      padding: 12px 16px;
      font-size: 14px;
      border: none;
      background: $color_main;
      color: #fff;
      border-radius: 6px;
      cursor: pointer;

      &:hover {
        background: $color_main_deep;
      }
    }
  }
}

/* =========================================================
   조회 결과 카드
========================================================= */
.info-card {
  background: #f9fbfb;
  border: 1px solid #e5e8e8;
  border-radius: 8px;
  padding: 24px 28px;
  margin-bottom: 20px;

  h2 {
    font-size: 18px;
    font-weight: 700;
    margin-bottom: 16px;
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0 0 20px;

    li {
      font-size: 15px;
      color: #444;
      margin-bottom: 8px;

      strong {
        color: #222;
        font-weight: 600;
        margin-right: 8px;
      }
    }
  }

  .btn.primary.full {
    width: 100%;
    padding: 14px 0;
    font-size: 15px;
    font-weight: 600;
    background: $color_main;
    color: #fff;
    border: none;
    border-radius: 6px;
    cursor: pointer;

    &:hover {
      background: $color_main_deep;
    }
  }
}

/* =========================================================
   하단 안내
========================================================= */
.hint {
  text-align: center;
  margin-top: 10px;
  font-size: 14px;
  color: #666;

  a {
    color: $color_main;
    text-decoration: underline;
    margin-left: 4px;

    &:hover {
      color: $color_main_deep;
    }
  }
}

/* =========================================================
   반응형
========================================================= */
@media (max-width: 768px) {
  .page-frame {
    border-width: 6px;
    border-radius: 16px;
    margin: 20px;
    padding: 20px 0;
  }

  .change-wrap {
    padding: 40px 16px;
  }

  /* ✅ header 부분 완전 보완 */
  .header {
    flex-direction: column;
    align-items: center;
    text-align: center;
    gap: 12px;

    .logo {
      width: 160px;
      margin-bottom: 8px;
    }

    h1 {
      font-size: 22px;
      line-height: 1.3;
    }

    .sub {
      font-size: 14px;
      line-height: 1.4;
    }
  }

  .content {
    padding: 30px 24px;
  }
}
</style>
