<template>
  <div class="join-page">
    <!-- ✅ 상단 영역 -->
    <header class="header">
      <div class="logo left"><img src="/public/images/mains/header/logo-1.png" alt="로고"></div>
      <h1>회원가입</h1>
      <div class="logo right"><img src="/public/images/mains/header/logo-1.png" alt="로고"></div>
    </header>

    <!-- ✅ 메인 카드 -->
    <div class="join-card">
      <form>
        <div class="form-group">
          <label>휴대폰 번호</label>
          <div class="input-row">
            <input type="text" placeholder="번호를 입력해 주세요" />
            <button type="button" class="btn small">인증 요청</button>
          </div>
          <p class="desc">-없이 입력</p>
        </div>

        <div class="form-group">
          <label>인증 번호</label>
          <div class="input-row">
            <input type="text" placeholder="인증번호" />
            <button type="button" class="btn small">인증 하기</button>
          </div>
          <p class="desc">번호를 입력해 주세요</p>
        </div>

        <div class="terms">
          <div class="term-header">
            <label><input type="checkbox" /> 전체 동의합니다</label>
          </div>
          <ul>
            <li><label><input type="checkbox" /> [필수] 만 14세 이상입니다.</label></li>
            <li><label><input type="checkbox" /> [필수] 서비스 이용약관 동의 (보기)</label></li>
            <li><label><input type="checkbox" /> [필수] 개인정보 수집 및 이용동의 (보기)</label></li>
            <li><label><input type="checkbox" /> [필수] 개인정보 제3자 제공동의 (보기)</label></li>
            <li><label><input type="checkbox" /> [선택] 마케팅 목적 개인정보 수집 및 이용동의 (보기)</label></li>
          </ul>
        </div>

        <button type="submit" class="btn primary full" @click="goToLogin">이용동의</button>
      </form>
    </div>
  </div>

<!-- ==================================================== -->
</template>
<script setup>
import { useRouter } from "vue-router";
import Signup2 from "@/views/sign/Signup2.vue";
// useRouter() 이동할때 사용하는 함수
const router = useRouter();
const goToLogin = () => {
  alert("동의완료");
  router.push("/signup2");
};
</script>
<style scoped lang="scss">
@use "/src/assets/style/variables" as *;

/*  전체 페이지 구조 */
.join-page {
  min-height: 100vh;
  background: #fff;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  z-index: 0;
  padding-bottom: 3rem;
}

/* 상단 헤더 */
.header {
  position: relative;
  height: 200px;
  width: 100%;
  background: $color_main;
  overflow: hidden; // 기울어진 패턴이 튀어나오지 않게
  text-align: center;
  padding: 50px 0;
  // z-index: 1;

  h1 {
    position: relative;
    z-index: 999;
    color: #333;
    font-size: 26px;
    font-weight: 700;
  }

 
}

/* 회원가입 카드 */
.join-card {
  background: #fff;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.06);
  padding: 50px 60px;
  width: 500px;
  position: relative;
  z-index: 10;
  margin-top: -60px;

  form {
    position: relative;
    z-index: 10;
  }
}

/* 라인형 입력 폼 */
.form_group {
  margin-bottom: 25px;

  label {
    font-size: 14px;
    font-weight: 600;
    color: #333;
    display: block;
    margin-bottom: 6px;
  }

  input,
  select {
    width: 100%;
    border: none;
    border-bottom: 1px solid #e7e7e7;
    background: transparent;
    font-size: 14px;
    padding: 10px 4px;
    outline: none;
    color: #333;
    transition: border-color 0.2s ease;

    &:focus {
      border-bottom: 1px solid #53b4a1;
    }
  }

  select {
    appearance: none;
    background: url("data:image/svg+xml;utf8,<svg fill='%2355b4a1' height='10' viewBox='0 0 20 20' width='10'><path d='M5 7l5 5 5-5H5z'/></svg>") 
      no-repeat right 10px center;
    background-size: 12px;
    padding-right: 28px;
  }

  .label {
    font-size: 12px;
    color: #888;
    margin-top: 4px;
  }
}

/* 새로운 입력 구조 (.form-group: 복수행 대응) */
.form-group {
  margin-bottom: 25px;

  label {
    font-size: 14px;
    font-weight: 600;
    color: #333;
    display: block;
    margin-bottom: 6px;
  }

  .input-row {
    display: flex;
    gap: 10px;

    input {
      flex: 1;
      border: none;
      border-bottom: 1px solid #ccc;
      font-size: 14px;
      padding: 8px 4px;
      outline: none;

      &:focus {
        border-bottom-color: $color_main_background;
      }
    }
  }

  .desc {
    font-size: 12px;
    color: #999;
    margin-top: 4px;
  }
}

/* 약관 영역 */
.terms {
  border-top: 1px solid #e7e7e7;
  padding-top: 15px;
  margin-top: 10px;

  .term-header {
    margin-bottom: 10px;
    label {
      font-weight: 700;
    }
  }

  ul {
    list-style: none;
    padding: 0;
    margin: 0;

    li {
      font-size: 13px;
      color: #555;
      margin-bottom: 6px;
    }
  }

  input[type="checkbox"] {
    margin-right: 8px;
    accent-color: $color_main_light;
  }
}

/* 버튼 */
.btn {
  background: $color_main;
  color: #fff;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-weight: 600;
  padding: 12px 16px;

  &.small {
    padding: 8px 14px;
    font-size: 13px;
  }

  &.primary.full {
    width: 100%;
    padding: 16px 0;
    font-size: 15px;
    margin-top: 25px;
  }

  &:hover {
    background: $color_main_deep;
  }
}

/* ✅ 반응형 */
@media (max-width: 600px) {
  .join-card {
    width: 90%;
    padding: 30px 20px;
  }
  .header .logo {
    display: none;
  }
}
</style>
