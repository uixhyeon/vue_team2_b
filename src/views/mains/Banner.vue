<template>
  <section class="banner-outer">
    <div class="banner-center">
      <swiper
        class="mbannerSwiper"
        :modules="[Autoplay, Navigation]"
        :autoplay="{ delay: 2500, disableOnInteraction: false }"
        :navigation="true"
        :loop="true"
      >
        <!-- slide 1 -->
        <swiper-slide>
          <div class="banner-grid">
            <div class="main-banner-txt">
              <h1 class="mb-txt1">QR 코드 출입</h1>
              <h2 class="mb-txt2">한 번의 스캔, 바로 입장</h2>
              <h3 class="mb-txt3">이용은 간단하게, 보안은 확실하게 보장합니다.</h3>
            </div>
            <div class="main-banner-img">
              <img src="/public/images/mains/banner/bannerimg1.png" alt="mainbannerimg" />
            </div>
          </div>
        </swiper-slide>

        <!-- slide 2 -->
        <swiper-slide>
          <div class="banner-grid">
            <div class="main-banner-txt">
              <h1 class="mb-txt1">안전한 보관</h1>
              <h2 class="mb-txt2">전문가의 안심 관리</h2>
              <h3 class="mb-txt3">전문가의 손길로 짐을 꼼꼼하게 관리합니다.</h3>
            </div>
            <div class="main-banner-img">
              <img src="/public/images/mains/banner/bannerimg2.png" alt="mainbannerimg" />
            </div>
          </div>
        </swiper-slide>

        <!-- side 3 -->
<swiper-slide>
          <div class="banner-grid">
            <div class="main-banner-txt">
              <h1 class="mb-txt1">가벼운 여행</h1>
              <h2 class="mb-txt2">답답한 짐 없이, 편리하게</h2>
              <h3 class="mb-txt3">당일 접수/보관도 가능합니다.</h3>
            </div>
            <div class="main-banner-img">
              <img src="/public/images/mains/banner/bannerimg3.png" alt="mainbannerimg" />
            </div>
          </div>
        </swiper-slide>
      </swiper>
    </div>
    <QuickR class="quickr" />
  </section>
</template>

<script setup>
import { Swiper, SwiperSlide } from "swiper/vue";
import "swiper/css";
import "swiper/css/navigation";
import { Autoplay, Navigation } from "swiper/modules";
import QuickR from "./QuickR.vue";
</script>

<style lang="scss" scoped>
.banner-outer { position: relative; }
.quickr{
  position:absolute; left:50%; bottom:-35px; transform:translatex(-50%);
  width:calc(100% - 48px); max-width:1320px; z-index:20; pointer-events:none;
}

/* 배경 */
.banner-outer{ width:100%; background:#f6f6f6; }
.banner-center{ max-width:1920px; margin:0 auto; position:relative; }

/* Swiper */
.mbannerSwiper{ width:100%; height:100%; }
:deep(.swiper-button-prev), :deep(.swiper-button-next){ color:#b3b3b3; display: none !important;}

/* ======================= ≥1000px ======================= */
@media (min-width:1000px){
  .banner-grid{
    display:grid;
    grid-template-columns:
      clamp(520px, 33.5vw, 640px)
      clamp(360px, 22vw, 416px);
    column-gap:clamp(40px, 3.3vw, 64px);
    align-items:center; justify-content:center;
    width:100%; max-width:clamp(960px, 58vw, 1120px);
    margin:0 auto;
    min-height:clamp(276px, 27.6vw, 530px);
  }

  /* 데스크톱: 좌측 레일 고정(시작점 동일) */
  .main-banner-txt{
    display:flex; flex-direction:column; gap:clamp(8px, .9vw, 12px);
    line-height:1.2;
    align-items:flex-start;
    text-align:left;
  }
  .main-banner-txt > *{ width:100%; margin-left:0; padding-left:0; text-indent:0; }

  .mb-txt1{ font-size:clamp(16px, 3vw, 26px); color:#3e9c9b; font-weight:600; margin:0; }
  .mb-txt2{ font-size:clamp(40px, 3.3vw, 64px); font-weight:800; color:#000; line-height:.7; margin:0; letter-spacing:-.02em; }
  .mb-txt3{ font-size:clamp(14px, 3vw, 20px); color:#000; font-weight:400; line-height:2; margin:0; }

  .main-banner-img{ display:flex; align-items:center; justify-content:flex-end; }
  .main-banner-img img{ width:100%; height:auto; object-fit:contain; display:block; }
}

/* ======================= <1000px ======================= */
@media (max-width:999.98px){
  .banner-center{ padding:24px 16px 32px; }

  .banner-grid{
    width:100%; max-width:720px; margin:0 auto;
    display:flex; flex-direction:column; align-items:center; row-gap:20px;
  }

  /* 모바일/태블릿: 예전처럼 중앙정렬 */
  .main-banner-txt{
    max-width:560px; width:100%;
    display:flex; flex-direction:column; gap:10px; line-height:1.2;
    align-items:center;            /* 중앙 배치 */
    text-align:center;             /* 텍스트 중앙정렬 */
  }
  .main-banner-txt > *{ margin:0; padding:0; text-indent:0; }

  .mb-txt1{ font-size:18px; color:#3e9c9b; font-weight:600; }
  .mb-txt2{ font-size:34px; font-weight:800; color:#000; line-height:.7; }
  .mb-txt3{ font-size:16px; color:#000; font-weight:400; line-height:1.25; }

  .main-banner-img img{
    width:min(85vw, 360px); height:auto; object-fit:contain; display:block;
  }

  :deep(.swiper-button-prev), :deep(.swiper-button-next){ width:28px; height:28px; }
  :deep(.swiper-button-prev){ left:16.1px; }
  :deep(.swiper-button-next){ right:16px; }
}
</style>
