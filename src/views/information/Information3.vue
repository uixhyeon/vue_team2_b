<template>
  <div class="info-inner">
    <section class="dont-list">
      <div class="dont-head">
        <h3 class="dont-title">보관 불가 품목</h3>
        <p class="dont-sub">이런 품목은 보관이 어렵습니다.</p>
      </div>

      <ul class="subject-wrap">
        <!-- cash -->
        <li class="ban-list">
          <span class="ban-icon">
            <svg viewBox="0 0 640 512" aria-hidden="true">
              <path
                d="M352 288h-16v-88c0-4.42-3.58-8-8-8h-13.58c-4.74 0-9.37 1.4-13.31 4.03l-15.33 10.22a7.994 7.994 0 0 0-2.22 11.09l8.88 13.31a7.994 7.994 0 0 0 11.09 2.22l.47-.31V288h-16c-4.42 0-8 3.58-8 8v16c0 4.42 3.58 8 8 8h64c4.42 0 8-3.58 8-8v-16c0-4.42-3.58-8-8-8zM608 64H32C14.33 64 0 78.33 0 96v320c0 17.67 14.33 32 32 32h576c17.67 0 32-14.33 32-32V96c0-17.67-14.33-32-32-32zM48 400v-64c35.35 0 64 28.65 64 64H48zm0-224v-64h64c0 35.35-28.65 64-64 64zm272 192c-53.02 0-96-50.15-96-112 0-61.86 42.98-112 96-112s96 50.14 96 112c0 61.87-43 112-96 112zm272 32h-64c0-35.35 28.65-64 64-64v64zm0-224c-35.35 0-64-28.65-64-64h64v64z"
              />
            </svg>
          </span>
          <p class="ban-text">공공기관에서는 보관함에 <strong>현금</strong>을 넣어두라고 요구하지 않습니다.</p>
        </li>

        <!-- 2. 음식물 -->
        <li class="ban-list">
          <span class="ban-icon">
            <svg viewBox="0 0 448 512" aria-hidden="true">
              <path
                d="M352 111.1c22.09 0 40-17.88 40-39.97S352 0 352 0s-40 49.91-40 72S329.9 111.1 352 111.1zM224 111.1c22.09 0 40-17.88 40-39.97S224 0 224 0S184 49.91 184 72S201.9 111.1 224 111.1zM383.1 223.1L384 160c0-8.836-7.164-16-16-16h-32C327.2 144 320 151.2 320 160v64h-64V160c0-8.836-7.164-16-16-16h-32C199.2 144 192 151.2 192 160v64H128V160c0-8.836-7.164-16-16-16h-32C71.16 144 64 151.2 64 160v63.97c-35.35 0-64 28.65-64 63.1v68.7c9.814 6.102 21.39 11.33 32 11.33c20.64 0 45.05-19.73 52.7-27.33c6.25-6.219 16.34-6.219 22.59 0C114.1 348.3 139.4 367.1 160 367.1s45.05-19.73 52.7-27.33c6.25-6.219 16.34-6.219 22.59 0C242.1 348.3 267.4 367.1 288 367.1s45.05-19.73 52.7-27.33c6.25-6.219 16.34-6.219 22.59 0C370.1 348.3 395.4 367.1 416 367.1c10.61 0 22.19-5.227 32-11.33V287.1C448 252.6 419.3 223.1 383.1 223.1zM352 373.3c-13.75 10.95-38.03 26.66-64 26.66s-50.25-15.7-64-26.66c-13.75 10.95-38.03 26.66-64 26.66s-50.25-15.7-64-26.66c-13.75 10.95-38.03 26.66-64 26.66c-11.27 0-22.09-3.121-32-7.377v87.38C0 497.7 14.33 512 32 512h384c17.67 0 32-14.33 32-32v-87.38c-9.91 4.256-20.73 7.377-32 7.377C390 399.1 365.8 384.3 352 373.3zM96 111.1c22.09 0 40-17.88 40-39.97S96 0 96 0S56 49.91 56 72S73.91 111.1 96 111.1z"
              />
            </svg>
          </span>
          <p class="ban-text">
            <strong>음식물</strong> 보관 시 부패나 악취로 인해 사전에 경고 없이 폐기처분될 수 있습니다.
          </p>
        </li>

        <!-- 3. 귀중품 -->
        <li class="ban-list">
          <span class="ban-icon">
            <svg id="icons" viewBox="0 0 512 512" aria-hidden="true">
              <polygon points="396.31 32 264 32 348.19 144.26 396.31 32" />
              <polygon points="115.69 32 163.81 144.26 248 32 115.69 32" />
              <polygon points="256 74.67 192 160 320 160 256 74.67" />
              <polygon points="422.95 51.06 376.26 160 488 160 422.95 51.06" />
              <polygon points="89.05 51.06 23 160 135.74 160 89.05 51.06" />
              <polygon points="146.68 192 24 192 246.8 480 247.33 480 146.68 192" />
              <polygon points="365.32 192 264.67 480 265.2 480 488 192 365.32 192" />
              <polygon points="329.39 192 182.61 192 256 400 329.39 192" />
            </svg>
          </span>
          <p class="ban-text">도난 우려가 있는 <strong>귀중품</strong>은 보관이 어렵습니다.</p>
        </li>

        <!-- 4. 위험물 -->
        <li class="ban-list">
          <span class="ban-icon">
            <svg viewBox="0 0 384 512" aria-hidden="true">
              <path
                d="M216 23.86c0-23.8-30.65-32.77-44.15-13.04C48 191.85 224 200 224 288c0 35.63-29.11 64.46-64.85 63.99-35.17-.45-63.15-29.77-63.15-64.94v-85.51c0-21.7-26.47-32.23-41.43-16.5C27.8 213.16 0 261.33 0 320c0 105.87 86.13 192 192 192s192-86.13 192-192c0-170.29-168-193-168-296.14z"
              />
            </svg>
          </span>
          <p class="ban-text">
            폭발 위험이 있는 <strong>위험물</strong><small> (ex. 라이터, 부탄가스 등)</small> 보관이 어렵습니다.
          </p>
        </li>

        <!-- 5. 생명체 -->
        <li class="ban-list">
          <span class="ban-icon">
            <svg viewBox="0 0 24 24" aria-hidden="true">
              <path d="M0 0H24V24H0z" fill="none" />
              <path
                d="M21 3v2c0 3.866-3.134 7-7 7h-1v1h5v7c0 1.105-.895 2-2 2H8c-1.105 0-2-.895-2-2v-7h5v-3c0-3.866 3.134-7 7-7h3zM5.5 2c2.529 0 4.765 1.251 6.124 3.169C10.604 6.51 10 8.185 10 10v1h-.5C5.358 11 2 7.642 2 3.5V2h3.5z"
              />
            </svg>
          </span>
          <p class="ban-text">동물과 식물 등 호흡을 하는 모든 <strong>생명체</strong>는 보관이 불가합니다.</p>
        </li>
      </ul>

      <div class="ban-note">
        <p class="ban-note1">※ 보관 불가 품목을 보관해서 일어난 일에 대해서 책임을 지지 않습니다.</p>
        <p class="ban-note1">※ 안내된 물건 외에 보관이 필요한 품목은 고객센터에 문의해 주세요.</p>
      </div>
    </section>
  </div>
</template>

<script setup></script>
<style scoped>
.info-inner {
  box-sizing: border-box;
  max-width: 995px;
  width: 100%;
  margin: 0 auto;
}
/* dont */
.dont-list {
  max-width: 1200px;
  margin: 110px auto;
}

.dont-head {
  margin-bottom: 40px;
}

.dont-title {
  font-size: 32px;
  color: #000;
  font-weight: 800;
}
.dont-sub {
  font-size: 18px;
  color: #555353;
}

.subject-wrap {
  list-style: none;
  margin: 0px 0 12px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}
.ban-list {
  max-width: 1100px;
  display: flex;
  align-items: center;
  background-color: #fff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
  gap: 30px;
  border-radius: 20px;
  padding: 15px 18px;
}

.ban-icon {
  flex: 0 0 auto;
  width: 50px;
  height: 50px;
  border-radius: 9999px;
  display: grid;
  place-items: center;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.08);
}
.ban-icon svg {
  width: 30px;
  height: 30px;
  fill: #3e9c9b;
}

.ban-text {
  font-size: 18px;
  font-weight: 500;
  line-height: 1.6;
}
.ban-text strong {
  font-weight: 800;
  color: #000;
}
.ban-text small {
  color: #b8b3b3;
  font-size: 15px;
}
.ban-note {
  margin-top: 20px;
  text-align: left;
}
.ban-note1 {
  font-size: 14px;
  color: #555353;
  font-weight: 400;
}

@media (max-width: 1290px) {
  .info-inner {
    max-width: 900px;
  }

  .dont-title {
    font-size: 30px;
  }

  .dont-sub {
    font-size: 17px;
  }

  .ban-list {
    padding: 14px 16px;
    gap: 16px;
  }

  .ban-text {
    font-size: 16px;
  }

  .ban-icon {
    width: 46px;
    height: 46px;
  }

  .ban-icon svg {
    width: 26px;
    height: 26px;
  }
}

/* 768px */
@media (max-width: 768px) {
  .dont-list {
    margin: 80px auto;
  }

  .dont-title {
    font-size: 26px;
  }

  .dont-sub {
    font-size: 15px;
  }

  .subject-wrap {
    gap: 10px;
  }

  .ban-list {
    flex-direction: row;
    align-items: center;
    text-align: center;
    gap: 14px;
    padding: 14px;
    border-radius: 16px;
  }

  .ban-icon {
    width: 42px;
    height: 42px;
  }

  .ban-icon svg {
    width: 24px;
    height: 24px;
  }

  .ban-text {
    font-size: 15px;
    text-align: center;
  }

  .ban-note1 {
    font-size: 13px;
  }
}

@media (max-width: 390px) {
  .dont-list {
    padding: 0;
  }
  .ban-list {
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;
    padding: 18px;
    gap: 10px;
    border-radius: 12px;
  }

  .ban-icon {
    width: 34px;
    height: 34px;
    flex-shrink: 0; /* 아이콘 줄어들지 않게 */
  }

  .ban-icon svg {
    width: 20px;
    height: 20px;
  }
  .dont-title {
    font-size: 26px;
  }
  .dont-sub {
    font-size: 16px;
  }
  .ban-text {
    font-size: 15px;
    line-height: 1.45;
    text-align: left;
  }

  .ban-text strong {
    font-size: 15px;
  }

  .ban-text small {
    font-size: 12px;
  }
  .ban-note1 {
    font-size: 12px;
    white-space: nowrap;
  }
}
</style>
