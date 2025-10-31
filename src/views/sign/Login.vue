<template>
  <div class="login-page">
    <!-- 왼쪽: 로그인 폼 -->
    <div class="login-left">
      <h1 class="title">마타주</h1>

      <form class="login-form" @submit.prevent="handleLogin">
        <input type="text" placeholder="아이디" v-model="userId" />
        <input type="password" placeholder="비밀번호" v-model="password" />
        <!-- ✅ 로그인 -->
        <button type="submit" class="login-btn">로그인</button>
        <!-- ✅ 비회원 예약하기: 폼 submit 안 타게 type="button" + 이동만 -->
        <button type="button" class="login-btn-2" @click="goGuestReserve">
          비회원 예약하기
        </button>

        <!-- 찾기 -->
        <div class="link-row">
          <a href="#" @click.prevent="showFindId = true">아이디 찾기</a>
          <span>|</span>
          <a href="#" @click.prevent="showFindPw = true">비밀번호 찾기</a>
          <span>|</span>
          <a href="/signup">회원가입</a>
        </div>
      </form>

      <div class="social-login">
        <button class="naver" aria-label="네이버 로그인">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="40"
            height="30"
            viewBox="0 0 600 500"
          >
            <path
              fill="#ffffff"
              d="M176 128h88l72 112V128h88v256h-88l-72-112v112h-88V128z"
            />
          </svg>
        </button>

        <button class="kakao"><i class="fa-solid fa-comment"></i></button>
        <button class="google"><i class="fa-brands fa-google"></i></button>
      </div>
    </div>

    <!-- 오른쪽: 쿠폰 배너 -->
    <div class="login-right">
      <div class="coupon-banner">
        <div class="banner-text">
          <div class="title-bg">
            <h2>마타주가 처음인 당신을 위해</h2>
          </div>
          <div class="scr-bg">
            <p>
              신규회원 가입 쿠폰을 발송드려요<br />
              가벼운 여행을 시작해 보세요!
            </p>
          </div>
        </div>
        <div class="coupon-card">
          <img src="/public/images/sign/Login_cupon.png" alt="쿠폰" />
        </div>
      </div>
    </div>
  </div>

  <!-- ✅ 모달 -->
  <FindIDModal v-if="showFindId" @close="showFindId = false" />
  <FindPwModal v-if="showFindPw" @close="showFindPw = false" />
</template>

<script setup>
import { ref } from "vue";
import { useRouter } from "vue-router";
import FindIDModal from "@/views/sign/FindID.vue";
import FindPwModal from "@/views/sign/FindPw.vue";
import FindResarv from "./FindResarv.vue";

const router = useRouter();

const userId = ref("");
const password = ref("");

// Find id pw
const showFindId = ref(false);
const showFindPw = ref(false);

const handleLogin = () => {
  if (!userId.value || !password.value) {
    alert("아이디와 비밀번호를 입력하세요.");
    return;
  }
  alert(`로그인 시도: ${userId.value}`);
};

// ✅ 비회원 예약하기 -> /reservation 으로 바로 이동
const goGuestReserve = () => {
  router.push("/reservation");
};
</script>

<style scoped lang="scss">
@use "/src/assets/style/variables" as *;

.coupon-card {
  width: 100%;
  img {
    width: 100%;
  }
}
.title-bg {
  // background-color: #2db400;
}
.scr-bg {
  // background-color: #ffe812;
  display: inline-block;
}
/* 메인 레이아웃: flex 기반 */
.login-page {
  display: flex;
  justify-content: center;
  align-items: stretch; /* 좌우 높이 동일하게 */
  min-height: 60vh;
  max-width: 1120px;
  gap: 2.5rem;
  margin: 0 auto;
  margin-bottom: 40px;
  gap: 0; /* 카드 사이 여백 조정 가능 */

  /* ✅ 반응형 (900px 이하일 때 1열 전환) */
  @media (max-width: 900px) {
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
  }
}

/* 왼쪽 로그인 폼 */
.login-left {
  // flex: 1;
  width: 400px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  // padding: 40px 20px;
  margin-top: 10px;
}

.title {
  font-size: 28px;
  color: $color_main;
  font-weight: 700;
  margin-bottom: 30px;
}

/* 로그인 폼 */
.login-form {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 320px;
  gap: 15px;

  input {
    width: 100%;
    padding: 14px 16px;
    border: 1px solid #e7e7e7;
    border-radius: 6px;
    font-size: 14px;
    outline: none;
    transition: border-color 0.2s ease;

    &:focus {
      border-color: $color_main_light;
    }
  }

  .login-btn {
    width: 100%;
    background: $color_main;
    color: #fff;
    border: none;
    border-radius: 6px;
    font-weight: 600;
    padding: 14px 0;
    cursor: pointer;
    font-size: 16px;
    transition: background 0.2s ease;
  

    &:hover {
      background: $color_main_deep;
    }
  }

  .login-btn-2 {
    width: 100%;
    background: rgb(201, 201, 201);
    color: #616161;
    border: none;
    border-radius: 6px;
    font-weight: 600;
    padding: 14px 0;
    cursor: pointer;
        font-size: 16px;

    transition: background 0.2s ease;

    &:hover {
      background: rgb(180, 180, 180);
    }
  }

  .link-row {
    display: flex;
    justify-content: center;
    align-items: center;
    gap: 6px;
    margin-top: 10px;
    font-size: 13px;
    color: #666;

    a {
      color: #666;
      text-decoration: none;
      transition: color 0.2s ease;

      &:hover {
        color: $color_main_deep;
      }
    }
  }
}

/* SNS 로그인 */
.social-login {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 14px;
  margin-top: 25px;

  button {
    width: 45px;
    height: 45px;
    border-radius: 50%;
    border: none;
    cursor: pointer;
    font-size: 20px;
    display: flex;
    align-items: center;
    justify-content: center;
  }

  .naver {
    background: #2db400;
    color: #fff;
  }

  .kakao {
    background: #ffe812;
    color: #3c1e1e;
  }

  .google {
    background: #fff;
    border: 1px solid #ddd;
    color: #333;
  }
}

/* 오른쪽 쿠폰 배너 */
.login-right {
  // flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  // padding: 60px 20px;
  // background: #f9fbfb;
  margin-top: 40px;
}

/* 배너 내용 */
.coupon-banner {
  width: 100%;
  background: linear-gradient(
      rgba(255, 255, 255, 0.8),
      rgba(255, 255, 255, 0.4)
    ),
    /* 위에 흐린 흰색 필터 */ url(/public/images/sign/2462865.png) no-repeat center
      center / cover;
  padding: 40px;
  border-radius: 6px;
  border: 1px solid #e7e7e7;
  text-align: center;
  max-width: 400px;
  background-color: #fff;

  .banner-text {
    h2 {
      font-size: 18px;
      color: #222;
      margin-bottom: 10px;
      font-weight: 700;
    }
    p {
      font-size: 14px;
      color: #555;
      line-height: 1.5;
      margin-bottom: 30px;
    }
  }

  .coupon-card {
    width: 100%;
    position: relative;
    color: #fff;
    border-radius: 6px;
    text-align: center;

    h1 {
      font-size: 32px;
      font-weight: 700;
      margin: 0;
    }

    p {
      font-size: 14px;
      color: #ccc;
      margin-top: 8px;
    }

    .side {
      position: absolute;
      top: 50%;
      right: -28px;
      transform: translateY(-50%) rotate(90deg);
      font-size: 11px;
      letter-spacing: 1px;
      color: #fff;
      opacity: 0.7;
    }
  }
}

@media (max-width: 480px) {
  .coupon-banner {
    width: 100%;
    background: linear-gradient(
        rgba(255, 255, 255, 0.8),
        rgba(255, 255, 255, 0.4)
      ),
      /* 위에 흐린 흰색 필터 */ url(/public/images/sign/2462865.png) no-repeat
        center center / cover;
    padding: 40px;
    border-radius: 6px;
    border: 1px solid #e7e7e7;
    text-align: center;
    max-width: 320px;
    background-color: #fff;
  }
}
</style>
