<template>
  <div class="cs-wrap" :class="{ 'no-scroll': showModal }">
    <h1 class="title">고객센터</h1>

    <!-- 카드 섹션 -->
    <div class="cards">
      <!-- 이용방법 -->
      <button class="card" @click="goTo('/information')">
        <div class="icon">
          <img src="/images/faq/faq-1.png" alt="이용방법 아이콘" />
        </div>
        <div class="card-tit">이용방법이 궁금하신가요?</div>
        <div class="card-sub">이용안내 보러가기</div>
      </button>

      <!-- 이용요금 -->
      <button class="card" @click="goTo('/information')">
        <div class="icon">
          <img src="/images/faq/faq-2.png" alt="이용요금 아이콘" />
        </div>
        <div class="card-tit">이용요금이 궁금하신가요?</div>
        <div class="card-sub">이용요금 보러가기</div>
      </button>

      <!-- 문의하기 -->
      <button class="card" @click="openInquiry">
        <div class="icon">
          <img src="/images/faq/faq-3.png" alt="문의하기 아이콘" />
        </div>
        <div class="card-tit">다른 문의가 있으신가요?</div>
        <div class="card-sub">문의하기</div>
      </button>
    </div>

    <!-- FAQ -->
    <div class="faq-head">자주 묻는 질문</div>

    <!-- 탭 -->
    <div class="tabs">
      <button
        v-for="t in tabs"
        :key="t.key"
        class="tab"
        :class="{ active: activeTab === t.key }"
        @click="handleTabClick(t.key)"
      >
        {{ t.label }}
      </button>
    </div>

    <!-- FAQ 리스트 -->
    <div class="faq-list">
      <div
        v-for="(item, i) in pagedFaqs"
        :key="item.id"
        class="faq-item"
        :class="{ open: openIndex === i }"
      >
        <button class="faq-q" @click="toggle(i)">
          <span>{{ item.q }}</span>
          <svg viewBox="0 0 24 24" class="chev">
            <path :d="openIndex === i ? upPath : downPath" />
          </svg>
        </button>
        <div v-show="openIndex === i" class="faq-a">
          {{ item.a }}
        </div>
      </div>
    </div>

    <!-- 페이지네이션 -->
    <div class="pager">
      <button class="ghost" @click="goPrev">이전</button>
      <span class="page-indicator">{{ currentPage }} / {{ totalPages }}</span>
      <button class="ghost" @click="goNext">다음</button>
    </div>

    <!-- ===== 문의하기 모달 ===== -->
    <div
      v-if="showModal"
      class="modal-backdrop"
      @click.self="closeInquiry"
      @keydown.esc="closeInquiry"
    >
      <div
        class="modal"
        role="dialog"
        aria-modal="true"
        aria-labelledby="inquiryTitle"
      >
        <h2 id="inquiryTitle" class="modal-title">문의하기</h2>

        <div class="field">
          <label for="inq-title" class="label">문의 제목</label>
          <input
            id="inq-title"
            ref="titleRef"
            v-model="titleInput"
            type="text"
            class="input"
            :maxlength="30"
            placeholder="제목을 입력해 주세요 (최대 30자)"
            @blur="touchTitle = true"
          />
          <!-- 제목 에러 -->
          <p v-if="titleError" class="error" aria-live="polite">{{ titleError }}</p>
        </div>

        <div class="field">
          <label for="inq-body" class="label">문의 내용</label>
          <div class="textarea-wrap">
            <textarea
              id="inq-body"
              v-model="bodyInput"
              class="textarea"
              :maxlength="500"
              placeholder="문의 내용은 최소 10자 이상 작성해 주세요."
              @focus="guardBodyFocus"
            ></textarea>
            <div class="counter">{{ bodyLen }}/500</div>
          </div>
        </div>

        <div class="btn-row">
          <button class="btn ghost" @click="closeInquiry">닫기</button>
          <button
            class="btn primary"
            :disabled="!canSubmit"
            @click="submitInquiry"
          >
            등록하기
          </button>
        </div>

        <!-- 성공 토스트 (모달 중앙) -->
        <div v-if="toastOn" class="modal-toast" role="status" aria-live="polite">
          {{ toastMsg }}
        </div>
      </div>
    </div>
    <!-- ===== /문의하기 모달 ===== -->
  </div>
</template>

<script setup>
import { ref, computed, onMounted, nextTick, watch } from 'vue'
import { useRouter } from 'vue-router'
const router = useRouter()

function goTo(path) {
  router.push(path)
}

/* 탭 정의 */
const tabs = [
  { key: 'all', label: '전체' },
  { key: 'reserve', label: '예약' },
  { key: 'ship', label: '배송' },
  { key: 'service', label: '서비스' },
  { key: 'pay', label: '결제' },
  { key: 'cancel', label: '취소/환불' },
]
const activeTab = ref('all')

/* FAQ 데이터 */
const allFaqs = [
  { id: 1, cat: 'service', q: '보관 가능한 짐의 종류에는 어떤 것이 있나요?', a: '일반 박스, 의류류, 소형 가전 등 일반 물품 보관이 가능합니다.' },
  { id: 2, cat: 'reserve', q: '보관 기간은 최대 얼마까지 가능한가요?', a: '기간 제한 없이 장기 보관이 가능합니다.' },
  { id: 3, cat: 'service', q: '보관 공간에 CCTV나 보안 시스템이 있나요?', a: '24시간 CCTV 및 출입 통제 시스템을 운영합니다.' },
  { id: 4, cat: 'ship', q: '해외로도 배송이 가능한가요?', a: '현재는 국내 배송만 지원합니다.' },
  { id: 5, cat: 'ship', q: '배송 시간은 지정할 수 있나요?', a: '희망 시간대 지정이 가능하며 지역에 따라 상이합니다.' },
  { id: 6, cat: 'ship', q: '주말이나 공휴일에도 배송이 되나요?', a: '지역·상황에 따라 일부 제한될 수 있습니다.' },
  { id: 7, cat: 'cancel', q: '예약 취소나 환불은 어떻게 하나요?', a: '마이페이지에서 취소 요청 가능하며 정책에 따라 환불됩니다.' },
  { id: 8, cat: 'service', q: '영업시간이 어떻게 되나요?', a: '평일 09:00~18:00 / 토 09:00~13:00 (일·공휴일 휴무).' },
  { id: 9, cat: 'pay', q: '결제 수단은 어떤 것들이 있나요?', a: '신용/체크카드, 간편결제를 지원합니다.' },
  { id: 10, cat: 'reserve', q: '예약 변경은 어떻게 하나요?', a: '예약 상세에서 날짜/시간 변경이 가능합니다.' },
  { id: 11, cat: 'service', q: '현장 방문 상담도 가능한가요?', a: '사전 예약 후 가능하며 일부 지역에 한정됩니다.' },
  { id: 12, cat: 'cancel', q: '부분 취소가 가능한가요?', a: '진행 상태에 따라 가능 여부가 달라집니다.' },

  { id: 13, cat: 'reserve', q: '보관 예약은 어떻게 진행되나요?', a: '홈페이지 또는 모바일 앱을 통해 간편하게 예약 가능합니다.' },
  { id: 14, cat: 'reserve', q: '예약 후 변경은 몇 번까지 가능한가요?', a: '제한 없이 변경 가능하지만 배송 전까지만 가능합니다.' },
  { id: 15, cat: 'ship', q: '배송 중 상태를 확인할 수 있나요?', a: '마이페이지의 배송 조회 기능에서 실시간 추적이 가능합니다.' },
  { id: 16, cat: 'ship', q: '배송이 지연될 경우 어떻게 되나요?', a: '예상 배송일보다 지연 시 문자 또는 알림을 통해 안내드립니다.' },
  { id: 17, cat: 'pay', q: '현금 결제는 가능한가요?', a: '현재는 카드 및 간편결제만 지원합니다.' },
  { id: 18, cat: 'pay', q: '세금계산서 발급이 가능한가요?', a: '사업자 회원의 경우 결제 완료 후 발급 가능합니다.' },
  { id: 19, cat: 'cancel', q: '환불은 얼마나 걸리나요?', a: '결제 수단에 따라 영업일 기준 2~5일 소요됩니다.' },
  { id: 20, cat: 'cancel', q: '예약 후 부분 취소가 가능한가요?', a: '서비스 진행 상태에 따라 일부 취소가 가능합니다.' },
  { id: 21, cat: 'service', q: '보관 장소는 어디에 있나요?', a: '전국 주요 도시에 물류센터가 있으며, 예약 시 선택 가능합니다.' },
  { id: 22, cat: 'service', q: '보관품 파손 시 보상 기준은 어떻게 되나요?', a: '보험 기준에 따라 보상 처리되며, 고객센터로 문의 바랍니다.' },
  { id: 23, cat: 'ship', q: '도서산간 지역도 배송이 가능한가요?', a: '일부 지역은 추가 요금이 발생할 수 있습니다.' },
  { id: 24, cat: 'pay', q: '영수증은 어디서 확인하나요?', a: '결제 완료 후 마이페이지 > 결제 내역에서 확인 가능합니다.' },
]
/* 페이지네이션 */
const perPage = 8
const currentPage = ref(1)

const filteredFaqs = computed(() => {
  if (activeTab.value === 'all') return allFaqs
  return allFaqs.filter(f => f.cat === activeTab.value)
})
const totalPages = computed(() => Math.max(1, Math.ceil(filteredFaqs.value.length / perPage)))
const pagedFaqs = computed(() => {
  const start = (currentPage.value - 1) * perPage
  return filteredFaqs.value.slice(start, start + perPage)
})

/* 아코디언 */
const openIndex = ref(-1)
function toggle(i) {
  openIndex.value = openIndex.value === i ? -1 : i
}

/* 탭 클릭 */
function handleTabClick(key) {
  activeTab.value = key
  currentPage.value = 1
  openIndex.value = -1
}

/* 이전/다음 */
function goPrev() {
  currentPage.value = currentPage.value > 1 ? currentPage.value - 1 : totalPages.value
  openIndex.value = -1
}
function goNext() {
  currentPage.value = currentPage.value < totalPages.value ? currentPage.value + 1 : 1
  openIndex.value = -1
}

/* 아이콘 path */
const downPath = 'M6 9l6 6 6-6'
const upPath = 'M6 15l6-6 6 6'

/* ===== 문의하기 모달 상태/핸들러 ===== */
const showModal = ref(false)
const titleInput = ref('')
const bodyInput = ref('')
const titleRef = ref(null)

/* 에러/토스트 상태 */
const touchTitle = ref(false)
const titleError = ref('')
const toastOn = ref(false)
const toastMsg = ref('')

const bodyLen = computed(() => (bodyInput.value || '').length)
const canSubmit = computed(() =>
  (titleInput.value || '').trim().length > 0 &&
  (bodyInput.value || '').trim().length >= 10
)

function openInquiry() {
  showModal.value = true
  nextTick(() => {
    titleRef.value?.focus()
  })
}
function closeInquiry() {
  showModal.value = false
  titleError.value = ''
  touchTitle.value = false
  toastOn.value = false
}

/* 제목 입력 안 했는데 본문에 포커스하려고 하면 가드 */
function guardBodyFocus() {
  if ((titleInput.value || '').trim().length === 0) {
    titleError.value = '제목은 필수로 입력해야 합니다.'
    nextTick(() => titleRef.value?.focus())
  }
}

/* 제목 실시간 에러 해제 */
watch(titleInput, (v) => {
  if ((v || '').trim().length > 0) titleError.value = ''
})

function submitInquiry() {
  touchTitle.value = true
  if ((titleInput.value || '').trim().length === 0) {
    titleError.value = '제목은 필수로 입력해야 합니다.'
    nextTick(() => titleRef.value?.focus())
    return
  }
  if (!canSubmit.value) return

  // 실제 전송(API) 연결 지점
  // await post('/api/inquiry', { title: titleInput.value, body: bodyInput.value })

  // 입력값 초기화 (모달은 유지)
  titleInput.value = ''
  bodyInput.value = ''
  titleError.value = ''
  touchTitle.value = false

  // ✅ 성공 토스트: 두 줄, 두 번째 줄은 한 문장 그대로
  toastMsg.value = '등록 완료!\n답변은 추후 마이페이지에서 확인하세요.'
  toastOn.value = true
  setTimeout(() => (toastOn.value = false), 2200)
}

onMounted(() => {
  // ESC는 템플릿에서 처리
})
</script>

<style scoped>
.cs-wrap {
  max-width: 950px;
  margin: 0 auto;
  padding: 80px 16px 120px;
  font-family: "Pretendard", "Noto Sans KR", sans-serif;
  color: #222;
}
.cs-wrap.no-scroll { overflow: hidden; }

/* 타이틀 */
.title {
  text-align: center;
  font-size: 34px;
  font-weight: 700;
  margin-bottom: 30px;
  margin-top: -30px;
}

/* 카드 */
.cards {
  display: flex;
  justify-content: center;
  gap: 3%;
  margin-bottom: 80px;
}
.card {
  width: 290px;
  height: 290px;
  background: #fff;
  border: 1px solid #e6eae9;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  transition: transform .2s ease, box-shadow .2s ease;
  padding: 10% 2%;
}
.card:hover { transform: translateY(-3px); box-shadow: 0 8px 25px rgba(0,0,0,0.08); }

/* 아이콘 */
.icon {
  width: 90px;
  height: 90px;
  margin-bottom: 20px;
  display: flex;
  align-items: center;
  justify-content: center;
}
.icon img { width: 100%; height: 100%; object-fit: contain; }

/* 텍스트 */
.card-tit { font-size: clamp(18px, 1.5vw, 20px); font-weight: 700; margin-bottom: 8px; color: #000;}
.card-sub { font-size: 16.5px; color: #a0afaa; }

/* FAQ */
.faq-head {
  text-align: center;
  font-size: 30px;
  font-weight: 700;
  margin-bottom: 30px;
  color: #222;
}

/* 탭 */
.tabs {
  display: flex;
  justify-content: center;
  gap: 14px;
  margin-bottom: 40px;
}
.tab {
  font-size: clamp(16px, 2vw, 18px);
  font-weight: 500;
  border: 1px solid #dcdedc;
  border-radius: 6px;
  padding: 10px 28px;
  background: #fff;
  color: #6b7a76;
  transition: all 0.2s ease;
  cursor: pointer;
}
.tab:hover { border-color: #bfcac7; }
.tab.active { background: #3A8C88; color: #fff; border-color: #3e9c9b; font-weight: 600; }

/* FAQ 리스트 */
.faq-list {
  border: 1px solid #e8eeec;
  border-radius: 8px;
  overflow: hidden;
  background: #fff;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.04);
}
.faq-item { border-bottom: 1px solid #eef2f1; }
.faq-item:last-child { border-bottom: none; }

/* 질문 버튼 */
.faq-q {
  appearance: none;
  border: none;
  background: #fff;
  width: 100%;
  text-align: left;
  display: flex;
  align-items: center;
  justify-content: space-between;
  gap: 6px;
  padding: 22px 26px;
  font-size: clamp(15px, 2vw, 17px);
  color: #3d3d3d;
  cursor: pointer;
  transition: background 0.2s ease;
  font-weight: 600;
}
.faq-q:hover { background: #f8fbfa; }

/* 답변 */
.faq-a {
  padding: 18px 24px 20px;
  font-size: clamp(15px, 1.5vw, 16px);
  color: #657370;
  background: #fbfdfc;
  transition: all 0.25s ease;
  font-weight: 500;
}
.faq-item.open .faq-a { color: #575757; }

/* 아이콘 */
.chev {
  width: 18px;
  height: 18px;
  stroke: currentColor;
  stroke-width: 1.8;
  fill: none;
}

/* 페이지네이션 */
.pager {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 18px;
  margin-top: 30px;
}
.ghost {
  font-size: 14px;
  border: 1px solid #e2e5e4;
  background: #fff;
  padding: 8px 20px;
  border-radius: 4px;
  cursor: pointer;
  color: #000;
}
.ghost:hover { background: #f8fbfa; }
.page-indicator { font-size: 14px; color: #6b7a76; }

/* === 모달 스타일 === */
.modal-backdrop{
  position: fixed;
  inset: 0;
  background: rgba(0,0,0,.8);
  display: grid;
  place-items: center;
  z-index: 1000;
  padding: 16px;
}
.modal{
  position: relative;
  width: min(92vw, 420px);
  background: #fff;
  border-radius: 12px;
  box-shadow: 0 20px 50px rgba(0,0,0,.18);
  padding: 20px 18px 16px;
}
.modal-title{
  text-align: center;
  font-size: 22px;
  font-weight: 800;
  margin: 4px 0 16px;
}
.field{ margin-bottom: 14px; }
.label{
  display: block;
  font-size: 16px;
  font-weight: 700;
  color: #3e9c9b;
  margin-bottom: 8px;
}
.input{
  width: 100%;
  height: 42px;
  border: 1px solid #dfe6e4;
  border-radius: 8px;
  padding: 0 12px;
  font-size: 16px;
  outline: none;
  transition: border-color .15s ease, box-shadow .15s ease;
  font-family: "Pretendard", "Noto Sans KR", sans-serif;
}
.input:focus{ border-color:#7ec3bb; box-shadow: 0 0 0 3px rgba(126,195,187,.25); }

/* placeholder 폰트/크기/색 통일 */
.input::placeholder,
.textarea::placeholder{
  font-family: "Pretendard", "Noto Sans KR", sans-serif;
  font-size: 15px;
  color: #a7b2af;
}

.textarea-wrap{ position: relative; }
.textarea{
  width: 100%;
  min-height: 140px;
  border: 1px solid #dfe6e4;
  border-radius: 8px;
  padding: 12px;
  font-size: 14px;
  line-height: 1.6;
  outline: none;
  resize: vertical;
  transition: border-color .15s ease, box-shadow .15s ease;
  font-family: "Pretendard", "Noto Sans KR", sans-serif;
}
.textarea:focus{ border-color:#7ec3bb; box-shadow: 0 0 0 3px rgba(126,195,187,.25); }
.counter{
  position: absolute;
  right: 10px; bottom: 8px;
  font-size: 12px;
  color: #9aa6a3;
}

/* 에러 문구 */
.error{
  margin-top: 6px;
  font-size: 12px;
  color: #d94a4a;
}

.btn-row{
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 10px;
  margin-top: 8px;
}
.btn{
  height: 44px;
  border-radius: 8px;
  font-size: 15px;
  font-weight: 700;
  cursor: pointer;
  border: 1px solid #d7e1df;
  background: #fff;
  color: #2b2b2b;
  transition: filter .15s ease, background .15s ease;
}
.btn:hover{ filter: brightness(0.98); }
.btn.primary{
  background: #2b8c7f;
  border-color: #2b8c7f;
  color: #fff;
}
.btn.primary:disabled{ opacity: .55; cursor: not-allowed; }

/* 모달 중앙 토스트: \n만 줄바꿈, 줄내 개행 금지 */
.modal-toast{
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  background: #2b8c7f;
  color: #fff;
  padding: 16px 20px;
  border-radius: 10px;
  font-size: 16px;
  font-weight: 600;
  white-space: pre;          /* ← \n만 줄바꿈, 자동줄바꿈 금지 */
  word-break: keep-all;      /* ← 한글도 중간 줄바꿈 방지 */
  overflow-wrap: normal;     /* ← 강제 개행 방지 */
  overflow-x: auto;          /* ← 너무 길면 가로 스크롤 */
  box-shadow: 0 12px 28px rgba(0,0,0,.18);
  pointer-events: none;
  text-align: center;
}

/* 반응형 */
@media (max-width: 769px) {
  .cards { flex-direction: column; align-items: center; gap: 24px; }
  .card { width: 260px; height: 240px; }
  .tabs { flex-wrap: wrap; gap: 10px; }
  .tab { font-size: 17px; padding: 9px 20px; }

  .modal{ width: min(94vw, 420px); padding: 18px 14px 14px; }
}
</style>
