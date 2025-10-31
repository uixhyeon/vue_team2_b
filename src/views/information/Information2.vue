<template>
  <div class="info-inner">
    <section class="locker-sizes v2">
      <header class="ls-head">
        <h3>사물함 사이즈</h3>
        <p class="sub">용도에 맞는 사이즈를 선택하면 상세 정보를 보여드릴게요.</p>
      </header>

      <!-- 선택 바 -->
      <div class="size-strip" role="tablist" aria-label="사물함 사이즈 선택">
        <button
          v-for="(s, i) in sizes"
          :key="s.key"
          class="pill"
          :class="{ active: i === idx }"
          role="tab"
          :aria-selected="i === idx"
          @click="idx = i"
        >
          <span class="tt">{{ s.title }}</span>
          <span class="dim">{{ s.dim }}</span>
        </button>
      </div>

      <!-- 상세 카드 -->
      <div class="detail">
        <div class="media">
          <img :src="sizes[idx].img" :alt="sizes[idx].alt" />
        </div>

        <div class="spec">
          <h4 class="name">{{ sizes[idx].title }}</h4>

          <dl class="rows">
            <div><dt>내부 규격</dt><dd>{{ sizes[idx].dim }}</dd></div>
            <div><dt>대략 용량</dt><dd>{{ sizes[idx].volume }}</dd></div>
            <div class="row-price"><dt>1일 요금</dt><dd><strong class="price">{{ sizes[idx].price.day }}</strong></dd></div>
            <!-- <div class="row-price"><dt>1개월 요금</dt><dd><strong class="price">{{ sizes[idx].price.month }}</strong></dd></div> -->
            <div>
              <dt>추천 용도</dt>
              <dd>
                <ul class="chips">
                  <li v-for="c in sizes[idx].fits" :key="c">{{ c }}</li>
                </ul>
              </dd>
            </div>
          </dl>

          <!-- CTA -->
          <div class="ship-cta">
            <span class="q">배송을 추가하시나요?</span>
            <button
              class="link-cta"
              :aria-expanded="showFees"
              aria-controls="distanceTable"
              @click="toggleFees"
            >
              {{ showFees ? '거리별 요금표 닫기' : '거리별 요금표 보기' }}
            </button>
          </div>
        </div>
      </div>

      <!-- 거리별 요금표 -->
      <transition name="accordion">
        <div
          v-show="showFees"
          id="distanceTable"
          class="fee-table"
          role="region"
          aria-label="거리별 요금표"
        >
          <div class="fee-head">
            <strong>{{ sizes[idx].title }} · 거리별 요금표</strong>
            <span class="hint">권역 기준 요금입니다.</span>
          </div>

          <div class="table-wrap">
            <table class="fees">
              <thead>
                <tr>
                  <th scope="col">구분</th>
                  <th scope="col">크기/중량 한도</th>
                  <th scope="col">요금</th>
                </tr>
              </thead>
              <tbody>
                <!-- 모바일 공통 기준 -->
                <tr class="limit-inline">
                  <td colspan="3">
                    <span class="limit-text">{{ sizes[idx].fee.limit }}</span>
                  </td>
                </tr>

                <tr>
                  <td data-label="구분">동일 권역</td>
                  <td data-label="크기/중량 한도" rowspan="2" class="limit-cell">
                    {{ sizes[idx].fee.limit }}
                  </td>
                  <td data-label="요금">{{ sizes[idx].fee.sameZone }}</td>
                </tr>
                <tr>
                  <td data-label="구분">타 권역</td>
                  <td data-label="요금">{{ sizes[idx].fee.otherZone }}</td>
                </tr>
              </tbody>
            </table>
          </div>

          <!-- 참고 안내 -->
          <ul class="notes">
            <li><strong>무게 초과:</strong> 허용 무게 초과 시 1kg당 <b>1,000원</b> 부과</li>
            <li><strong>부피 초과:</strong> 기준 초과 시 자동 상위 등급 청구</li>
            <li><strong>승차/하차 환경:</strong> 엘리베이터 미설치, 계단 이동 등 특이사항 발생 시 추가 요금</li>
            <li><strong>특수 물품:</strong> 유리, 전자제품, 고가품 등은 별도 포장 및 취급 수수료 부과</li>
          </ul>
        </div>
      </transition>
    </section>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const sizes = [
  { key:'XS', title:'XS', dim:'35×55×30',
    img:'/images/information/info-xs.png', alt:'미니 사이즈',
    volume:'0.3m³', fits:['소형가전','서류','장난감'],
    price:{ day:'5,000원', month:'35,000원' },
    fee:{ limit:'~55cm / 15kg 이하', sameZone:'4,000원', otherZone:'5,500원' } },

  { key:'S', title:'S', dim:'45×70×70',
    img:'/images/information/info-s.png', alt:'스몰 사이즈',
    volume:'0.4m³', fits:['의류박스','서랍형수납'],
    price:{ day:'8,000원', month:'60,000원' },
    fee:{ limit:'~70cm / 30kg 이하', sameZone:'6,000원', otherZone:'8,000원' } },

  { key:'M', title:'M', dim:'170×115×240',
    img:'/images/information/info-m.png', alt:'미디움 사이즈',
    volume:'0.6m³', fits:['여행가방','의류박스'],
    price:{ day:'15,000원', month:'110,000원' },
    fee:{ limit:'~170cm / 50kg 이하', sameZone:'15,000원', otherZone:'18,000원' } },

  { key:'L', title:'L', dim:'155×175×240',
    img:'/images/information/info-l.png', alt:'라지 사이즈',
    volume:'2.1m³', fits:['행거의류','중형가전'],
    price:{ day:'20,000원', month:'150,000원' },
    fee:{ limit:'~175cm / 100kg 이하', sameZone:'20,000원', otherZone:'28,000원' } },

  { key:'XL', title:'XL', dim:'190×275×240',
    img:'/images/information/info-xl.png', alt:'맥스라지',
    volume:'2.9m³', fits:['대형짐','스포츠장비'],
    price:{ day:'28,000원', month:'200,000원' },
    fee:{ limit:'~275cm / 150kg 이하', sameZone:'28,000원', otherZone:'40,000원' } },
]

const idx = ref(0)
const showFees = ref(false)
function toggleFees(){ showFees.value = !showFees.value }
</script>

<style scoped>
.info-inner {
  box-sizing: border-box;
  max-width: 1100px;
  width: 100%;
  margin: 0 auto;
}
.locker-sizes.v2{ width:min(1100px,92vw); margin:0 auto; }
.ls-head h3{ margin:0; font-size:clamp(26px,2vw,32px); font-weight:800; color: #000;}
.ls-head .sub{ margin:6px 0 14px; color:#555353; font-size: clamp(16px,2vw,18px); white-space: nowrap;}

/* ====== 탭 ====== */
.size-strip{
  display:grid;
  grid-template-columns:repeat(auto-fit, minmax(150px, 1fr));
  gap:10px; padding:6px 2px 10px; margin-bottom:14px;
}
.pill{
  display:flex; flex-direction:column; justify-content:center; align-items:center;
  border:1px solid #e5e7eb; background:#fff; border-radius:999px;
  padding:10px 16px; cursor:pointer; transition:border-color .2s, background .2s;
  text-align:center; white-space:nowrap;
}
.pill .tt{ color:#000; font-weight: 800; font-size:clamp(13px,1.4vw,14px); line-height:1.3; }
.pill .dim{ color:#6b7280; font-size:12px; line-height:1.2; }
.pill.active{ border-color:#3A8C88; background:#f0fdfd; }
@media (max-width:640px){ .size-strip{ grid-template-columns:repeat(auto-fit, minmax(130px, 1fr)); } }

/* ====== 상세 카드(글자 +3px 업) ====== */
.detail{
  display:grid; grid-template-columns:minmax(280px, 360px) 1fr;
  gap:22px;
  padding:18px;
  border-radius:16px;
  background:#fff;
  border:1px solid #eef2f7;
  box-shadow: 0 2px 8px rgba(2, 6, 23, .04);
  align-items:center;
  padding-bottom: 30px;
}
.media{
  width:100%; max-width:360px; height:280px;
  background:linear-gradient(180deg,#fbfdff, #f4f7fb);
  border:1px solid #eef2f7;
  border-radius:12px; overflow:hidden;
  display:flex; justify-content:center; align-items:center;
  padding:12px; box-sizing:border-box;
}
.media img{ width:100%; height:100%; object-fit:contain; object-position:center; display:block; }

.spec .name{ margin:2px 0 12px; font-size:25px; font-weight:900; letter-spacing:-.01em; }
.rows{ display:grid; gap:10px; }
.rows > div{
  display:grid; grid-template-columns:110px 1fr; gap:10px; align-items:center;
}
.rows dt{ color:#667085; font-size:16px; font-weight:700; }
.rows dd{ margin:0; font-size:17px; color:#0f172a; }

/* 가격 강조 */
.row-price .price{ font-weight:600; color:#0f172a; }
.row-price dd{ display:flex; align-items:center; }

/* 추천 용도 */
.chips{ list-style:none; padding:0; margin:0; display:flex; flex-wrap:wrap; gap:8px; }
.chips li{
  padding:6px 10px; border-radius:999px; background:#f3f4f6; font-size:13px;
  border:1px solid #e5e7eb;
}

/* CTA 버튼 — 글자 +3px */
.ship-cta{ margin-top:10px; display:flex; align-items:center; gap:10px; font-size:17px; }
.ship-cta .q{ color:#0f172a; font-weight:800; white-space: nowrap;}
.link-cta{
  appearance:none; -webkit-appearance:none; -moz-appearance:none;
  border:0;
  background:#e6f9fb;
  color:#3e9c9b;
  font-weight:600;
  padding:10px 16px;
  line-height:1;
  border-radius:999px;
  cursor:pointer;
  box-shadow: inset 0 0 0 1.5px #99f6e4;
  white-space: nowrap;
}
.link-cta:hover{ background:#d9f4f7; box-shadow: inset 0 0 0 1.5px #7ee3e9; }
.link-cta:focus-visible{ outline:2px solid #22c1c7; outline-offset:2px; }

/* ====== 요금표 컨테이너 ====== */
.table-wrap{
  overflow-x:auto; border:1px solid #e6edf3; border-radius:14px; background:#fff;
  box-shadow: 0 1px 0 rgba(15,23,42,.03);
  margin-top:20px;
  margin-bottom:20px;
}
.fees{ width:max(560px,100%); border-collapse:separate; border-spacing:0; }
.fees thead th{
  background:#f8fafc; font-weight:800; color:#111827;
  padding:12px 14px; border-bottom:1px solid #eef2f7;
}
.fees th, .fees td{ padding:12px 14px; font-size:14px; text-align:left; }
.fees tbody td{ border-bottom:1px solid #f1f5f9; }
.fees tbody tr:last-child td{ border-bottom:0; }
.fees td[rowspan]{ vertical-align:middle; }

/* 모바일(≤600px): 카드형 + 공통 기준 분리 */
@media (max-width:600px){
  .detail{ grid-template-columns:1fr; }
  .media{ margin:0 auto; max-width:min(82vw, 360px); height:min(62vw, 260px); }

  .table-wrap{ overflow-x:visible; padding:8px; }
  .fees{ width:100%; }
  .fees thead{ display:none; }

  .limit-inline{ display:block; }
  .limit-inline td{
    padding:10px 12px !important;
    border:1px dashed #cbd5e1; border-radius:10px; background:#f8fafc;
  }
  .limit-inline .limit-text{ font-weight:700; color:#0f172a; }

  .fees tbody{ display:grid; gap:8px; justify-items: stretch; }
  .fees tbody tr{
    display:grid; grid-template-columns:1fr; gap:8px;
    border:1px solid #eef2f7; border-radius:12px; padding:12px; background:#fff;
  }
  .fees td{
    display:flex; justify-content:space-between; align-items:center;
    border-bottom:0; padding:0; font-size:14px;
  }
  .fees td::before{
    content: attr(data-label);
    font-weight:700; color:#475569; margin-right:10px;
  }
  .fees td[rowspan].limit-cell{ display:none; }
}
@media (min-width:601px){
  .limit-inline{ display:none; }
}

/* 참고 박스 */
.notes{
  position:relative; margin-top:30px; margin-bottom:30px; padding:14px 16px;
  background:#f8fafc; border:1px dashed #cbd5e1; border-radius:12px;
  color:#475569; font-size:14.5px; line-height:1.6; list-style:none;
}
.notes::before{
  content:"참고"; position:absolute; top:-10px; left:12px;
  background:#3e9c9b; color:#fff; font-weight:800; font-size:11px;
  padding:2px 8px; border-radius:999px; box-shadow:0 1px 0 rgba(0,0,0,.04);
}
.notes li+li{ margin-top:4px; }
.notes li{ position:relative; padding-left:14px; }
.notes li::before{ content:"•"; position:absolute; left:0; top:0; color:#3e9c9b; font-weight:700; }

/* 아코디언 */
.accordion-enter-from, .accordion-leave-to{ max-height:0; opacity:0; }
.accordion-enter-active, .accordion-leave-active{
  overflow:hidden; transition:max-height .18s ease, opacity .18s ease;
}

.fee-head{ margin-top: 30px; }

/* ====== 텍스트 전체적으로 확대 ====== */

/* 탭 버튼 */
.pill .tt {
  font-size: clamp(16px, 1.6vw, 17px); /* +3px */
  font-weight: 750;
}
.pill .dim {
  font-size: 14px; /* +2px */
}

/* 상세 정보 */
.spec .name {
  font-size: 25px; /* +3px */
}
.rows dt {
  font-size: 15px; /* +2px */
}
.rows dd {
  font-size: 16px; /* +2px */
  font-weight: 600;
}
.ship-cta {
  font-size: 16px; /* +2px */
}
.link-cta {
  font-size: 15px; /* +2px */
  padding: 10px 16px;
}

/* ====== 요금표 ====== */
.fees th,
.fees td {
  font-size: 16px; /* +2px */
  padding: 14px 16px;
}
.limit-inline .limit-text {
  font-size: 15px; /* +2px */
}

/* ====== 참고 박스 ====== */
.notes {
  font-size: 14.5px; /* 기존 12.5 → +1px */
  line-height: 1.7;
}
.notes::before {
  font-size: 12px; /* 조금 키움 */
}
.notes li::before {
  top: 1px;
}

/* ====== 반응형: 모바일에서도 비율 유지 ====== */
@media (max-width: 600px) {
  .fees td {
    font-size: 15px;
  }
  .fees td::before {
    font-size: 14px;
  }
}

</style>
