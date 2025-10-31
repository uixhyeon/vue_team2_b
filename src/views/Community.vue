<template>
  <!-- ====== WANTED 배너 (배경/텍스트는 코드, 포스터만 이미지) ====== -->
  <section class="wanted-banner">
    <div class="wb-inner">
      <div class="wb-copy">
        <p class="eyebrow">최대 <strong>35,000원</strong> 혜택</p>
        <h2 class="wb-headline">
          현상금 걸고<br />
          리뷰어를 찾습니다!
        </h2>
        <p class="wb-sub">
          포토/텍스트 리뷰를 남기면 적립금 보너스를 드려요.
        </p>
      </div>

      <!-- 우측 포스터(이미지) -->
      <div class="wb-poster">
        <img src="/images/community/review-banner-2.png" alt="">
      </div>
    </div>

    <!-- 배경(하늘/사막/바위/선인장) -->
    <div class="wb-bg">
      <div class="sky"></div>
      <div class="cloud c1"></div>
      <div class="cloud c2"></div>
      <div class="sun"></div>
      <div class="mesa left"></div>
      <div class="mesa right"></div>
      <div class="ground"></div>
      <div class="cactus"></div>
    </div>
  </section>

  <!-- ====== 본문(탭 + 콘텐츠) ====== -->
  <div class="review-wrap">
    <div class="container">
      <!-- Tabs -->
      <nav class="tabs" role="tablist" aria-label="review tabs">
        <button
          v-for="t in tabs"
          :key="t.key"
          class="tab"
          :class="{ active: activeTab === t.key }"
          @click="activeTab = t.key"
          role="tab"
          :aria-selected="activeTab === t.key"
        >
          {{ t.label }}
        </button>
        <span class="tab-ink" :style="inkStyle"></span>
      </nav>

      <!-- ========== 한줄 후기 ========== -->
      <section v-if="activeTab === 'reviews'" class="panel" aria-label="한줄 후기">
        <ul class="card-list">
          <li v-for="r in visibleReviews" :key="r.id" class="card">
            <header class="card-head">
              <div class="user">
                <div class="avatar" :style="{ backgroundColor: r.avatarColor }">{{ r.avatarEmoji }}</div>
                <div class="meta">
                  <div class="nick">{{ r.nickname }}</div>
                  <div class="sub">{{ r.badge }}</div>
                </div>
              </div>
              <div class="score">
                <svg viewBox="0 0 24 24" class="star" aria-hidden="true">
                  <path d="M12 17.27 18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z"/>
                </svg>
                <span class="score-text">{{ r.score.toFixed(1) }}</span>
              </div>
            </header>

            <p class="content">{{ r.text }}</p>

            <footer class="card-foot">
              <div class="actions">
                <button class="vote" :class="{ up: r.vote === 'up', active: r.vote === 'up' }" @click="toggleVote(r, 'up')">
                  <svg viewBox="0 0 24 24" class="icon"><path d="M2 21h4V9H2v12zM22 10c0-1.1-.9-2-2-2h-6.31l.95-4.57.03-.32c0-.41-.17-.79-.44-1.06L13 1 6.59 7.41C6.22 7.78 6 8.3 6 8.83V19c0 1.1.9 2 2 2h8c.83 0 1.54-.5 1.84-1.22l3.02-7.05c.09.23.14.47.14.73V10z"/></svg>
                  <span :class="{ pop: r.popAnim === 'up' }">{{ r.up }}</span>
                </button>
                <button class="vote" :class="{ down: r.vote === 'down', active: r.vote === 'down' }" @click="toggleVote(r, 'down')">
                  <svg viewBox="0 0 24 24" class="icon"><path d="M2 3h4v12H2V3zm20 11c0 1.1-.9 2-2 2h-6.31l.95 4.57.03.32c0 .41-.17.79-.44 1.06L13 23l-6.41-6.41C6.22 16.22 6 15.7 6 15.17V5c0-1.1.9-2 2-2h8c.83 0 1.54.5 1.84 1.22l3.02-7.05c.09.23.14.47.14.73V14z"/></svg>
                  <span :class="{ pop: r.popAnim === 'down' }">{{ r.down }}</span>
                </button>
              </div>
              <div class="meta2">
                <time :datetime="r.dateISO">{{ r.date }}</time>
                <span class="dot">·</span>
                <button class="link" @click.prevent>신고</button>
              </div>
            </footer>
          </li>
        </ul>

        <div class="loadmore-wrap" v-if="visibleCount < reviews.length">
          <!-- 숫자 제거 -->
          <button class="loadmore" @click="loadMore">더보기</button>
        </div>
      </section>

      <!-- ========== 질문과 답변 ========== -->
      <section v-else-if="activeTab === 'qna'" class="panel" aria-label="질문과 답변">
        <ul class="qna-list">
          <li v-for="q in visibleQnas" :key="q.id" class="qna-card">
            <header class="qna-head">
              <div class="user">
                <div class="avatar" :style="{ backgroundColor: q.avatarColor }">{{ q.avatarEmoji }}</div>
                <div class="meta"><div class="nick">{{ q.nickname }}</div></div>
              </div>
            </header>

            <p class="qna-title">{{ q.title }}</p>
            <p class="qna-text" v-if="q.text">{{ q.text }}</p>

            <footer class="qna-foot">
              <!-- SVG → 이모지 -->
              <button class="like" :class="{ on: q.liked }" @click="toggleLike(q)">
                <span class="emoji" aria-hidden="true">{{ q.liked ? '💖' : '🤍' }}</span>
                <span class="num">{{ q.likes }}</span>
              </button>
              <!-- ▼ 변경: 열림 상태 표시 + ARIA -->
              <button
                class="cmt"
                :class="{ on: q.open }"
                @click="toggleComments(q)"
                :aria-expanded="q.open"
                :aria-controls="'cbox-' + q.id"
              >
                <span class="emoji" aria-hidden="true">💬</span>
                <span>댓글</span>
                <strong>{{ q.comments.length }}</strong>
              </button>
            </footer>

            <!-- ▼ 변경: id 연결 -->
            <div v-if="q.open" class="cbox" :id="'cbox-' + q.id">
              <ul class="cmt-list" v-if="q.comments.length">
                <li v-for="(c, i) in q.comments" :key="i" class="cmt-item">
                  <div class="cmeta">
                    <span class="name">{{ c.name }}</span><span class="dot">·</span>
                    <time class="time">{{ c.time }}</time>
                  </div>
                  <p class="ctxt">{{ c.text }}</p>
                </li>
              </ul>
              <div class="editor">
                <textarea
                  v-model="q.draft"
                  placeholder="댓글을 입력해주세요."
                  maxlength="500"
                  @input="updateCount(q)"
                />
                <div class="editor-foot">
                  <span class="count">{{ q.count }}/500</span>
                  <button class="submit" :disabled="!canSubmit(q)" @click="submitComment(q)">등록</button>
                </div>
              </div>
            </div>
          </li>
        </ul>

        <div class="loadmore-wrap" v-if="qnaVisibleCount < qnas.length">
          <!-- 숫자 제거 -->
          <button class="loadmore" @click="loadMoreQna">더보기</button>
        </div>
      </section>

      <!-- ========== 포토 리뷰 ========== -->
      <section v-else class="panel" aria-label="포토 리뷰">
        <!-- 래퍼: 흐림 오버레이 + 높이 제한 -->
        <div class="photo-grid-wrap" :class="{ collapsed: !photoExpanded }">
          <div
            ref="gridRef"
            class="photo-grid"
            :style="{ maxHeight: photoExpanded ? 'none' : collapsedHeight + 'px' }"
          >
            <div v-for="n in 9" :key="n" class="ph">
              <img
                :src="`/images/community/comu-${n}.png`"
                :alt="`포토 리뷰 ${n}`"
                class="zoomable"
                tabindex="0"
                @click="openLightbox(n - 1)"
                @keydown.enter.prevent="openLightbox(n - 1)"
                @keydown.space.prevent="openLightbox(n - 1)"
              />
            </div>
          </div>
          <!-- 바닥 흐림 -->
          <div class="grid-fade" v-if="!photoExpanded"></div>
        </div>

        <!-- 포토 더보기 버튼 -->
        <div class="loadmore-wrap" v-if="!photoExpanded">
          <button class="loadmore" @click="expandPhotos">더보기</button>
        </div>
      </section>

      <!-- ===== Lightbox Modal ===== -->
      <teleport to="body">
        <div
          v-if="lightbox.open"
          class="lb-backdrop"
          role="dialog"
          aria-modal="true"
          :aria-label="images[lightbox.idx]?.alt || '이미지 미리보기'"
          @click.self="closeLightbox"
        >
          <button class="lb-close" @click="closeLightbox" aria-label="닫기">×</button>

          <button class="lb-nav prev" @click.stop="prev" aria-label="이전">‹</button>
          <figure class="lb-stage">
            <img :src="images[lightbox.idx].src" :alt="images[lightbox.idx].alt" />
            <!-- 인스타그램식 캡션 바 -->
            <figcaption class="lb-ig">
              <div class="ig-row">
                <strong class="ig-user">{{ captions[lightbox.idx].user }}</strong>
                <span class="ig-dot">·</span>
                <time class="ig-time">{{ captions[lightbox.idx].time }}</time>
              </div>
              <p class="ig-text">{{ captions[lightbox.idx].text }}</p>
            </figcaption>
          </figure>
          <button class="lb-nav next" @click.stop="next" aria-label="다음">›</button>
        </div>
      </teleport>
    </div>
  </div>
</template>

<script setup>
import { reactive, ref, computed, onMounted, onBeforeUnmount, nextTick, watchEffect } from 'vue'

/* 포스터 이미지 경로 (현재 고정 사용) */
const props = defineProps({
  posterSrc: { type: String, default: '' }
})
const posterSrc = props.posterSrc

/* Tabs */
const tabs = [
  { key: 'reviews', label: '한줄 후기' },
  { key: 'qna',     label: '질문과 답변' },
  { key: 'photos',  label: '포토 리뷰' }
]
const activeTab = ref('reviews')

const inkStyle = computed(() => {
  const idx = tabs.findIndex(t => t.key === activeTab.value)
  return { width: '33.3333%', transform: `translateX(${idx * 100}%)` }
})

/* Reviews */
const reviews = reactive([
  { id: 1, avatarEmoji: '🍯', avatarColor: '#FFE08C', nickname: '꿀템헌터', badge: '리뷰 4건', score: 4.9, text: '집 보관이 정말 편했어요! 기사님도 너무 친절하셨어요.', date: '3일 전',  dateISO: '2025-10-25', up: 23, down: 1, vote: null, popAnim: null },
  { id: 2, avatarEmoji: '📦', avatarColor: '#E6F4FF', nickname: '짐많은자', badge: '리뷰 6건', score: 5.0, text: '포장도 깔끔하고 보관 공간도 넉넉했어요. 재이용 의사 100%.', date: '5일 전',  dateISO: '2025-10-23', up: 14, down: 0, vote: null, popAnim: null },
  { id: 3, avatarEmoji: '🧳', avatarColor: '#F1F3F5', nickname: '여행러버', badge: '리뷰 2건', score: 4.8, text: '공항에서 맡기고 배송까지 한 번에! 시간 절약 최고.', date: '1주 전', dateISO: '2025-10-20', up: 9,  down: 2, vote: null, popAnim: null },
  { id: 4, avatarEmoji: '🏠', avatarColor: '#E8F5E9', nickname: '자취만렙', badge: '리뷰 3건', score: 4.9, text: '이사 기간 중 임시보관으로 딱이었어요.', date: '1주 전', dateISO: '2025-10-19', up: 7, down: 0, vote: null, popAnim: null },
  { id: 5, avatarEmoji: '🚚', avatarColor: '#FFF3E0', nickname: '이사러',   badge: '리뷰 1건', score: 4.7, text: '기사님 안내가 친절했고, 픽업 시간 정확했어요.', date: '9일 전', dateISO: '2025-10-18', up: 6, down: 1, vote: null, popAnim: null },
  { id: 6, avatarEmoji: '🗂️', avatarColor: '#E3F2FD', nickname: '정리장인', badge: '리뷰 5건', score: 5.0, text: '박스 라벨링 서비스가 특히 좋았습니다.', date: '10일 전', dateISO: '2025-10-17', up: 11, down: 0, vote: null, popAnim: null },
  { id: 7, avatarEmoji: '💼', avatarColor: '#FCE4EC', nickname: '워홀러',   badge: '리뷰 2건', score: 4.8, text: '장기보관 할인 덕분에 비용도 합리적이었어요.', date: '12일 전', dateISO: '2025-10-15', up: 8, down: 0, vote: null, popAnim: null },
  { id: 8, avatarEmoji: '🎒', avatarColor: '#EDE7F6', nickname: '백패커',   badge: '리뷰 3건', score: 4.9, text: '여행 짐 맡기고 가볍게 다녀왔습니다.', date: '2주 전', dateISO: '2025-10-13', up: 10, down: 1, vote: null, popAnim: null },
  { id: 9, avatarEmoji: '🖼️', avatarColor: '#FFFDE7', nickname: '수집러',   badge: '리뷰 8건', score: 5.0, text: '취급주의 물건도 안전하게 보관돼서 만족.', date: '2주 전', dateISO: '2025-10-12', up: 15, down: 0, vote: null, popAnim: null },
  { id:10, avatarEmoji: '🛠️', avatarColor: '#F1F8E9', nickname: '공구덕후', badge: '리뷰 2건', score: 4.8, text: '부피 큰 공구도 문제 없었어요.', date: '2주 전', dateISO: '2025-10-11', up: 4, down: 0, vote: null, popAnim: null },
  { id:11, avatarEmoji: '👟', avatarColor: '#F3E5F5', nickname: '스니커',   badge: '리뷰 7건', score: 4.9, text: '보관 환경이 쾌적해서 신발 변형 없음.', date: '3주 전', dateISO: '2025-10-08', up: 12, down: 1, vote: null, popAnim: null },
  { id:12, avatarEmoji: '📚', avatarColor: '#E0F7FA', nickname: '북덕후',   badge: '리뷰 2건', score: 4.7, text: '책 박스가 튼튼해요. 습기 걱정 없었습니다.', date: '3주 전', dateISO: '2025-10-07', up: 5, down: 0, vote: null, popAnim: null },
  { id:13, avatarEmoji: '🎹', avatarColor: '#FFF3E0', nickname: '피아니스트',badge: '리뷰 4건', score: 4.8, text: '악기 보관 온·습도 안내가 좋아요.', date: '3주 전', dateISO: '2025-10-06', up: 6, down: 0, vote: null, popAnim: null },
  { id:14, avatarEmoji: '🧥', avatarColor: '#E8EAF6', nickname: '코트모아', badge: '리뷰 1건', score: 5.0, text: '겨울옷 시즌 보관에 강추.', date: '4주 전', dateISO: '2025-10-02', up: 9, down: 0, vote: null, popAnim: null },
  { id:15, avatarEmoji: '🖥️', avatarColor: '#E0F2F1', nickname: '기기러버', badge: '리뷰 3건', score: 4.9, text: '전자제품도 포장 꼼꼼하게 처리해줍니다.', date: '4주 전', dateISO: '2025-10-01', up: 7, down: 1, vote: null, popAnim: null }
])

function toggleVote(r, dir) {
  if (r.vote === dir) {
    if (dir === 'up' && r.up > 0) r.up--
    if (dir === 'down' && r.down > 0) r.down--
    r.vote = null
  } else {
    if (r.vote === 'up' && r.up > 0) r.up--
    if (r.vote === 'down' && r.down > 0) r.down--
    r.vote = dir
    if (dir === 'up') r.up++; else r.down++
    r.popAnim = dir; setTimeout(() => (r.popAnim = null), 250)
  }
}
const VISIBLE_STEP = 5
const visibleCount = ref(VISIBLE_STEP)
const visibleReviews = computed(() => reviews.slice(0, visibleCount.value))
function loadMore() { visibleCount.value = Math.min(visibleCount.value + VISIBLE_STEP, reviews.length) }

/* QnA */
const qnas = reactive([
  { id: 1, avatarEmoji: '🐻', avatarColor: '#FFF3CD', nickname: '불박곰 가민', title: '첫보관인데 대여 서비스로 렌탈 의자 빌려본 분 계신가요?', text: '상태나 내구성이 괜찮았는지 궁금해요.', likes: 10, liked: false, comments: [{ name: '운영팀', time: '방금', text: '대여 품목은 출고 전 점검하며 상태 이슈 시 즉시 교체해 드립니다.' }], draft: '', count: 0, open: false },
  { id: 2, avatarEmoji: '🐯', avatarColor: '#FFF0E5', nickname: '침착한 범이라', title: '라이프존 파크 근처에서 방문 서비스 이용해 보신 분들 어땠나요?', text: '', likes: 3, liked: false, comments: [], draft: '', count: 0, open: false },
  { id: 3, avatarEmoji: '🦊', avatarColor: '#FFE8E8', nickname: '설레는 꾸밍이', title: '특정 짐장에서 보관함 크기나 단위 종류가 다른가요?', text: '자주 이동하시는 지점 추천 부탁드려요~', likes: 12, liked: true, comments: [{ name: '스텝A', time: '1일 전', text: '지점마다 라지/미디움 규격 동일합니다.' }, { name: '스텝B', time: '1일 전', text: '이동 많으시면 센터역점 추천드려요.' }], draft: '', count: 0, open: false },
  { id: 4, avatarEmoji: '🐥', avatarColor: '#E8FFF6', nickname: '안정한 흰메미', title: '보관 서비스 이용 시 카드 결제는 당일에만 가능한 건가요?', text: '간편결제도 가능한지 궁금합니다.', likes: 20, liked: false, comments: [{ name: '운영팀', time: '2일 전', text: '카카오/네이버페이 등 모든 간편결제 지원합니다.' }], draft: '', count: 0, open: false },
  { id: 5, avatarEmoji: '🌝', avatarColor: '#EEF4FF', nickname: '궁금한 얼룩달', title: '야간에도 픽업이 가능한가요?', text: '늦은 시간 픽업 요청이 잦아요. 안전 문제도 궁금해요.', likes: 7, liked: false, comments: [], draft: '', count: 0, open: false },
  { id: 6, avatarEmoji: '🥨', avatarColor: '#FFF5E6', nickname: '짓궂은 솔송이', title: '보관 공간에 CCTV가 설치되어 있나요?', text: '출입기기와 열람이 걱정됩니다.', likes: 11, liked: false, comments: [], draft: '', count: 0, open: false }
])
function toggleLike(q) { q.liked = !q.liked; q.likes += q.liked ? 1 : -1 }
function toggleComments(q) { q.open = !q.open }
function updateCount(q) { q.count = q.draft.length }
function canSubmit(q) { return q.draft.trim().length > 0 && q.count <= 500 }
function submitComment(q) {
  if (!canSubmit(q)) return
  q.comments.unshift({ name: '나', time: '방금', text: q.draft.trim() })
  q.draft = ''; q.count = 0
}
const QNA_STEP = 5
const qnaVisibleCount = ref(QNA_STEP)
const visibleQnas = computed(() => qnas.slice(0, qnaVisibleCount.value))
function loadMoreQna() { qnaVisibleCount.value = Math.min(qnaVisibleCount.value + QNA_STEP, qnas.length) }

/* ===== Lightbox state ===== */
const images = Array.from({ length: 9 }, (_, i) => ({
  src: `/images/community/comu-${i + 1}.png`,
  alt: `포토 리뷰 ${i + 1}`,
}))
const captions = [
  { user: 'lovelykim***', time: '방금',   text: '#오랜만에짐보따리 #언제나친절 #다음에도이용해야지 😊' },
  { user: 'packman***',   time: '1시간 전', text: '라커 깔끔! #편리함 #픽업굿' },
  { user: 'hiker_lee',    time: '어제',    text: '#여행짐보관 #가볍게다녀옴' },
  { user: 'toolguy',      time: '2일 전',  text: '#공구보관 #안전해요' },
  { user: 'wintercoat',   time: '2일 전',  text: '#겨울옷보관 #습도관리' },
  { user: 'booklover',    time: '3일 전',  text: '#책보관 #튼튼박스' },
  { user: 'snkrman',      time: '3일 전',  text: '#스니커보관 #형태유지' },
  { user: 'cellist',      time: '4일 전',  text: '#악기보관 #온습도안내' },
  { user: 'backpacker',   time: '5일 전',  text: '#여행끝 #편리' },
]

/* ===== Photo collapse (2.5 rows + fade) ===== */
const photoExpanded = ref(false)
const collapsedHeight = ref(0)
const gridRef = ref(null)
const GRID_GAP_PX = 10

function calcCollapsedHeight() {
  const grid = gridRef.value
  if (!grid) return
  const firstItem = grid.querySelector('.ph')
  if (!firstItem) return
  const rect = firstItem.getBoundingClientRect()
  const cell = rect.width
  const rows = 2.5
  const gaps = GRID_GAP_PX * 2
  collapsedHeight.value = cell * rows + gaps
}
function expandPhotos() {
  photoExpanded.value = true
  nextTick(() => calcCollapsedHeight())
}
function onResize() {
  if (!photoExpanded.value) calcCollapsedHeight()
}

// 탭 전환 시 photos 들어오면 계산
watchEffect(() => {
  if (activeTab.value === 'photos') nextTick(() => calcCollapsedHeight())
})

/* ===== Lightbox ===== */
const lightbox = reactive({ open: false, idx: 0 })
function openLightbox(i) {
  lightbox.idx = i
  lightbox.open = true
  document.documentElement.style.overflow = 'hidden'
}
function closeLightbox() {
  lightbox.open = false
  document.documentElement.style.overflow = ''
}
function next() { lightbox.idx = (lightbox.idx + 1) % images.length }
function prev() { lightbox.idx = (lightbox.idx - 1 + images.length) % images.length }

// ESC / 화살표 키
function onKey(e) {
  if (!lightbox.open) return
  if (e.key === 'Escape') closeLightbox()
  else if (e.key === 'ArrowRight') next()
  else if (e.key === 'ArrowLeft') prev()
}

// 전역 이벤트
onMounted(() => {
  window.addEventListener('keydown', onKey)
  window.addEventListener('resize', onResize)
})
onBeforeUnmount(() => {
  window.removeEventListener('keydown', onKey)
  window.removeEventListener('resize', onResize)
})
</script>

<style scoped>
/* ========================= WANTED 배너 ========================= */
.wanted-banner { position: relative; width: 100%; overflow: clip; isolation: isolate; }
.wb-inner {
  width: min(1200px, 92vw);
  min-height: clamp(200px, 18vw, 320px);
  margin: 0 auto; display: flex; align-items: center; justify-content: center;
  gap: clamp(16px, 5vw, 90px); padding: clamp(20px, 3vw, 32px) 0; position: relative; z-index: 2;
}
.wb-copy { color: #111; text-align: left; display: grid; gap: 8px; margin-top: 2%; }
.eyebrow { margin: 0; font-size: clamp(16px, 2vw, 20px); color: rgba(0,0,0,.65); }
.eyebrow strong { font-weight: 800; }
.wb-headline { margin: 0; font-weight: 900; line-height: 1.2; letter-spacing: -0.02em; font-size: clamp(25px, 3.2vw, 44px); }
.wb-sub { margin: 2px 0 0; color: rgba(0,0,0,.7); font-size: clamp(14px, 2vw, 18px); font-weight: 500; }

/* 포스터 영역 */
.wb-poster { flex: 0 0 auto; width: clamp(140px, 18vw, 240px); aspect-ratio: 3/5; display: grid; place-items: center; }
.wb-poster img { width: 100%; height: 100%; object-fit: contain; object-position: center; }

/* 배경(하늘/구름/해/사막/바위/선인장) */
.wb-bg { position: absolute; inset: 0; z-index: 1; }
.sky { position: absolute; inset: 0 0 35% 0; background: linear-gradient(#a8d8ff 0%, #bfe6ff 40%, #eaf6ff 100%); height: 80%; }
.sun {
  position: absolute; top: 18%; left: 55%;
  width: clamp(48px, 6vw, 80px); aspect-ratio: 1/1; border-radius: 50%;
  background: radial-gradient(circle at 35% 35%, #ffeab0 0 40%, #ffd36e 55%, #ffc04d 75%, #0000 76%);
  filter: blur(0.2px); opacity: .9;
}
.cloud { position: absolute; top: 18%; width: clamp(80px, 12vw, 160px); height: clamp(24px, 4vw, 48px); background: #fff; border-radius: 999px; opacity: .8; filter: blur(0.2px); }
.cloud.c1 { left: 8%; }
.cloud.c2 { left: 28%; top: 22%; width: clamp(60px, 10vw, 120px); opacity: .85; }
.ground { position: absolute; inset: auto 0 0 0; height: 20%; background: linear-gradient(#e8c07a, #d3a55e); border-top: 1px solid rgba(0,0,0,.06); }
.mesa {
  position: absolute; bottom: calc(35% - 2px);
  width: clamp(100px, 18vw, 260px); height: clamp(40px, 10vw, 120px);
  background: linear-gradient(#c97a3e, #a15d2c);
  clip-path: polygon(10% 100%, 10% 60%, 25% 30%, 40% 45%, 55% 20%, 70% 45%, 85% 30%, 92% 60%, 92% 100%);
  opacity: .7; filter: drop-shadow(0 2px 0 rgba(0,0,0,.03));
}
.mesa.left  { left: 6%;  transform: scaleX(1); }
.mesa.right { right: 6%; transform: scaleX(-1); }
.cactus { position: absolute; left: 12%; bottom: 18%; width: 8px; height: 64px; background: #2f8b57; border-radius: 4px; }
.cactus::before, .cactus::after { content: ""; position: absolute; background: #2f8b57; border-radius: 4px; }
.cactus::before { left: -16px; top: 18px; width: 6px; height: 28px; }
.cactus::after  { right: -16px; top: 26px; width: 6px; height: 24px; }

/* 768px 이하 세로 정렬 & 중앙 정렬 */
@media (max-width: 768px) {
  .wb-inner { flex-direction: column; text-align: center; }
  .wb-copy { text-align: center; }
}

/* ========================= 본문 레이아웃(기존) ========================= */
.review-wrap { width: 100%; display: flex; justify-content: center; }
.container {
  width: 680px; margin: 0 auto;
  display: flex; flex-direction: column; align-items: center;
  font-family: 'Pretendard','Apple SD Gothic Neo','Noto Sans KR',system-ui,-apple-system,Segoe UI,Roboto,Arial,sans-serif;
  color: #222;
}

/* Tabs */
.tabs { position: relative; width: 100%; display: flex; margin: 16px 0 16px; border-bottom: 1px solid #eee; }
.tab { flex: 1 0 0; height: 44px; line-height: 44px; text-align: center; font-size: 20px; font-weight: 600; color: #666; background: transparent; border: 0; cursor: pointer; }
.tab.active { color: #111; }
.tab-ink { position: absolute; bottom: -1px; left: 0; height: 2px; background: #111; width: 33.3333%; transition: transform .2s ease; }

/* Panel */
.panel { width: 100%; margin: 0 auto; padding-top: 8px; }

/* Cards */
.card-list { list-style: none; padding: 0; margin: 0; display: grid; gap: 12px; }
.card { border: 1px solid #eee; border-radius: 12px; padding: 16px 18px; background: #fff; }
.card-head { display: flex; align-items: center; justify-content: space-between; gap: 12px; }
.user { display: flex; align-items: center; gap: 12px; }
.avatar { width: 36px; height: 36px; border-radius: 10px; display: grid; place-items: center; font-size: 18px; }
.meta .nick { font-size: 15px; font-weight: 700; line-height: 1.1; }
.meta .sub  { margin-top: 3px; font-size: 12px; color: #8a8a8a; }
.score { display: inline-flex; align-items: center; gap: 6px; }
.star { width: 18px; height: 18px; fill: #FFC83D; }
.score-text { font-size: 14px; font-weight: 700; }
.content { margin: 12px 0 10px; font-size: 16px; line-height: 1.6; color: #222; word-break: keep-all; }

/* Card foot */
.card-foot { display: flex; align-items: center; justify-content: space-between; }
.actions { display: inline-flex; gap: 8px; }
.vote { display: inline-flex; align-items: center; gap: 6px; height: 32px; padding: 0 10px; font-size: 13px; border: 1px solid #E5E7EB; border-radius: 8px; background: #fff; cursor: pointer; transition: transform .08s ease, border-color .15s ease, background .15s ease; }
.vote .icon { width: 16px; height: 16px; fill: currentColor; }
.vote.up.active   { color: #0EA5E9; border-color: #BAE6FD; background: #F0F9FF; }
.vote.down.active { color: #EF4444; border-color: #FECACA; background: #FEF2F2; }
.vote:active { transform: scale(0.98); }
.vote span { display: inline-block; min-width: 12px; }
.vote span.pop { animation: pop .22s ease; }
@keyframes pop { 50% { transform: scale(1.2); } }
.meta2 { font-size: 12px; color: #8a8a8a; display: inline-flex; align-items: center; gap: 6px; }
.meta2 .link { background: none; border: 0; color: #8a8a8a; text-decoration: underline; cursor: pointer; }
.meta2 .dot { opacity: .5; }

/* Load more */
.loadmore-wrap { display: grid; place-items: center; margin-top: 12px; }
.loadmore { min-width: 160px; height: 40px; padding: 0 16px; border-radius: 10px; border: 1px solid #E5E7EB; background: #fff; font-size: 14px; font-weight: 600; cursor: pointer; }
.loadmore:hover { background: #F9FAFB; }
.loadmore:active { transform: translateY(1px); }

/* QnA */
.qna-list { list-style: none; padding: 0; margin: 0; display: grid; gap: 12px; }
.qna-card { border: 1px solid #EEE; background: #fff; border-radius: 12px; padding: 14px 16px; }
.qna-head { display: flex; align-items: center; justify-content: space-between; }
.qna-title { margin: 8px 0 4px; font-size: 17px; font-weight: 700; line-height: 1.5; }
.qna-text { margin: 0 0 8px; font-size: 16px; color: #333; line-height: 1.6; }
.qna-foot { display: flex; gap: 10px; align-items: center; }

/* 이모지 버튼 공통 */
.like, .cmt {
  display: inline-flex;
  align-items: center;
  gap: 6px;
  height: 32px;
  padding: 0 10px;
  font-size: 13px;
  line-height: 1;
  border: 1px solid #E5E7EB;
  background: #fff;
  border-radius: 8px;
  cursor: pointer;
}
.like.on { color: #E11D48; border-color: #FECACA; background: #FEF2F2; }

/* 댓글 토글 on 상태 표시 */
.cmt { transition: background .15s ease, border-color .15s ease, color .15s ease; }
.cmt.on {
  color: #0EA5E9;
  border-color: #BAE6FD;
  background: #F0F9FF;
  box-shadow: 0 0 0 2px rgba(186,230,253,.35) inset;
}
.cmt.on .emoji { transform: translateY(1px) scale(1.1); }

/* 이모지 크기/정렬 보정 */
.like .emoji, .cmt .emoji {
  font-size: 16px;
  line-height: 1;
  display: inline-block;
  transform: translateY(1px);
}
.cmt strong { font-weight: 700; }

/* 댓글 박스 */
.cbox { margin-top: 10px; border: 1px solid #EEF2F7; background: #FAFBFC; border-radius: 10px; padding: 12px; }
.cmt-list { list-style: none; padding: 0; margin: 0 0 10px; display: grid; gap: 10px; }
.cmt-item { background: #fff; border: 1px solid #EEE; border-radius: 8px; padding: 10px 12px; }
.cmeta { font-size: 14px; color: #777; display: inline-flex; gap: 6px; align-items: center; margin-bottom: 4px; }
.cmeta .dot { opacity: .6; }
.ctxt { margin: 0; font-size: 14px; line-height: 1.6; white-space: pre-wrap; }
.editor textarea {
  width: 100%; min-height: 88px; resize: vertical;
  border-radius: 8px; border: 1px solid #E5E7EB; padding: 10px 12px;
  font-size: 14px; line-height: 1.6; outline: none; background: #fff;
}
.editor textarea:focus { border-color: #C7EBED; box-shadow: 0 0 0 3px rgba(0,163,165,.1); }
.editor-foot { display: flex; align-items: center; justify-content: space-between; margin-top: 8px; }
.count { font-size: 12px; color: #888; }
.submit { min-width: 76px; height: 36px; padding: 0 14px; font-weight: 700; border: none; border-radius: 999px; background: #12B3B6; color: #fff; cursor: pointer; }
.submit:disabled { background: #D1D5DB; cursor: not-allowed; }

/* ===== Photo (그리드 & 썸네일) ===== */
.photo-grid-wrap { position: relative; }
.photo-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 10px;
  justify-content: center;
  overflow: hidden;
  transition: max-height .25s ease;
}
@media (max-width: 600px){
  .photo-grid{ grid-template-columns: repeat(2, 1fr); }
}

.ph {
  aspect-ratio: 1 / 1;
  border-radius: 10px;
  background: #f3f4f6;
  border: 1px solid #eee;
  overflow: hidden;
  display: flex;
  justify-content: center;
  align-items: center;
}
.ph img { width: 100%; height: 100%; object-fit: cover; object-position: center; display: block; }

/* 흐림 오버레이 (접힘 상태에서만 표시) */
.grid-fade{
  position: absolute;
  left: 0; right: 0; bottom: 0;
  height: 80px;
  background: linear-gradient(to bottom, rgba(255,255,255,0), rgba(255,255,255,1));
  pointer-events: none;
  border-radius: 0 0 10px 10px;
}

/* 클릭 가능 표시 */
.zoomable { cursor: zoom-in; }

/* ===== Lightbox (중앙정렬 + IG 캡션) ===== */
.lb-backdrop{
  position: fixed; inset: 0; z-index: 9999;
  background: rgba(0,0,0,.7);
  display: grid; place-items: center;
  height: 100dvh;
  padding: clamp(12px, 4vw, 40px);
  backdrop-filter: blur(2px);
}
.lb-stage{
  margin: 0;
  display: inline-grid;
  gap: 0;
  width: fit-content;
  max-width: calc(min(92vw, 1100px) * 0.667);
  justify-items: stretch;
}
.lb-stage img{
  width: auto;
  height: auto;
  max-width: 100%;
  max-height: calc(78vh * 0.667);
  object-fit: contain;
  display: block;
  border-radius: 12px 12px 0 0;
  background: #111;
}

/* IG-style caption bar */
.lb-ig{
  width: 100%;
  display: grid; gap: 6px;
  padding: 10px 14px;
  background: #fff; color: #111;
  border-radius: 0 0 12px 12px;
  box-shadow: 0 6px 20px rgba(0,0,0,.08);
}
.ig-row{ display: inline-flex; gap: 8px; align-items: center; font-size: 13px; }
.ig-user{ font-weight: 700; font-size: 20px;}
.ig-dot{ opacity: .45; }
.ig-time{ color: #666; }
.ig-text{
  margin: 0; font-size: 16px; margin-bottom: 3px;
  white-space: nowrap; overflow: hidden; text-overflow: ellipsis; color: #0a56fa;
}

.lb-close{
  position: fixed; top: 14px; right: 16px;
  width: 36px; height: 36px; border-radius: 50%;
  border: 0; background: rgba(255,255,255,.15); color: #fff;
  font-size: 22px; cursor: pointer;
}
.lb-close:hover{ background: rgba(255,255,255,.25); }

.lb-nav{
  position: absolute; top: 50%; transform: translateY(-50%);
  width: 44px; height: 44px; border-radius: 50%;
  border: 0; background: rgba(255,255,255,.15); color: #fff;
  font-size: 28px; cursor: pointer;
}
.lb-nav:hover{ background: rgba(255,255,255,.25); }
.lb-nav.prev{ left: 16px; }
.lb-nav.next{ right: 16px; }

@media (max-width: 768px){
  .lb-stage{ max-width: calc(92vw * 0.8); }
  .lb-stage img{ max-height: calc(74vh * 0.8); }
  .wb-inner{ gap: 0; }
}

.review-wrap{
  padding: 15px;
  margin: 10px 0 20px;
}

/* 투표/공통 버튼 정렬 안정화 */
.vote, .like, .cmt {
  line-height: 1;
}
</style>
