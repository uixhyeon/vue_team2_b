<template>

<!-- ==================================================== -->
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
          <div class="form_group">
            <label>이메일 아이디*</label>
            <input type="text" placeholder="Matajuu @ amail.com" />
            <p class="label">회원가입 신청후 받음 메일에서 확인 해야 완료됩니다</p>
          </div>

          <div class="form_group">
            <label>비밀번호*</label>
            <input type="text" placeholder="영문 숫자 포함 8글자 이상" />
            <p class="label">일치하지 않습니다</p>
          </div>
          <div class="form_group">
            <label>비밀번호 확인*</label>
            <input type="text" placeholder="비밀번호를 입력해 주세요" />
            <p class="label">일치하지 않습니다</p>
          </div>
          
          <div class="title-wrap">
            <h2>선택입력 정보</h2>
            <!-- <p class="label">짐 배송 및 예약시에 사용됩니다</p> -->
      </div>
          <div class="form_group">
            <label>성함*</label>
            <input type="text" placeholder="성함을 입력해주세요" />
            <p class="label">5자 이하</p>
          </div>

          <div class="form_group">
            <label>주소*</label>
            <input type="text" placeholder="지번 및 도로명 주소를 입력해주세요" />
            <p class="label">배송시 사용됩니다</p>
            <div class="gapp"></div>
            <input type="text" placeholder="상세주소를 입력해주세요" />
            <p class="label">상세주소~~</p>
          </div>

             <button type="submit" class="btn primary full" @click="goToLogin">입력 완료</button>
        </div>
      </form>
    </div>
  </div>
</template>
<script setup>
import { useRouter } from "vue-router";
// useRouter() 이동할때 사용하는 함수
const router = useRouter();
const goToLogin = () => {
  alert("회원가입완료");
  router.push("/login");
};
</script>

<style scoped lang="scss">
@use "/src/assets/style/variables" as *;

.title-wrap{
display: flex;

  
    width: 100%;
    border: none;
    border-bottom: 2px solid #999999;
    background: transparent;
    font-size: 14px;
    // padding: 10px 4px;
    outline: none;
    color: #333;
    transition: border-color 0.2s ease;

    &:focus {
      border-bottom: 1px solid $color_main_light;
    }
  

}

/* 전체 페이지 구조 */
.join-page {
  min-height: 100vh;
  background: #f5f7f7;
  display: flex;
  flex-direction: column;
  align-items: center;
  position: relative;
  z-index: 0;
}
//*  상단 헤더 */
.header {
  position: relative;
  height: 200px;
  width: 100%;
  background: #59B5B3; /* 단색 배경 */
  overflow: hidden;
  text-align: center;
  padding: 70px 0;
  z-index: 1;

  /* 회원가입 텍스트 복구 + 명확한 색상 */
  h1 {
    position: relative;
    z-index: 3;             /* 로고보다 위로 */
    color: #fff;            
    font-size: 28px;
    font-weight: 700;
    margin: 0;
  }

 
  .logo {
    position: absolute;
    top: 50%;
    transform: translateY(-50%) rotate(45deg);
    width: 70px;
    height: 70px;
    z-index: 2;

    img {
      width: 100%;
      height: 100%;
      opacity: 0.12; /* ✅ 아주 은은하게 (너무 눈에 띄지 않게) */
      object-fit: contain;
    }

    &.left {
      left: 80px;
    }

    &.right {
      right: 80px;
    }
  }
}

/*  회원가입 카드 */
.join-card {
  background: #fff;
  box-shadow: 0 3px 10px rgba(0, 0, 0, 0.06);
  // border-radius: 4px;
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

/*  라인형 입력 폼 */
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

/* ✅ 새로운 입력 구조 (.form-group: 복수행 대응) */
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
        border-bottom-color: $color_main_light;
      }
    }
  }

  .desc {
    font-size: 12px;
    color: #999;
    margin-top: 4px;
  }
}

/* ✅ 약관 영역 */
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

/* ✅ 버튼 */
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
.gapp{
  height: 6px;
}
// ========================라인으로 ============
.title-wrap {
  display: flex;
  flex-direction: column;
  gap: 2px;
  margin: 30px 0 15px;
  padding-bottom: 8px;
  border-bottom: 1px solid #e7e7e7;   // ✅ 기존 입력라인과 동일한 색상
  width: 100%;

  h2 {
    font-size: 16px;
    font-weight: 700;
    color: #333;
    margin: 0;
  }

  .label {
    font-size: 12px;
    color: #888;
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
