<template>
  <!-- 모달 구조 통일 버전 -->
    @@@@@@@@@@@@@@@@@지우기@@@@@@@@@@@@@@@@@@@@@@@
<div
  v-if="showModal"
  class="addr-modal"
  role="dialog"
  aria-modal="true"
  aria-labelledby="branch-title"
  @click="closeModal"
>
  <div class="dialog" @click.stop>
    <!-- 헤더 -->
    <div class="header">
      <h3 id="branch-title">지점 선택</h3>
      <button class="icon-btn" @click="closeModal" aria-label="닫기">✕</button>
    </div>

    <!-- 콘텐츠 -->
    <div class="content">
      <div class="modal-body">
        <!-- 왼쪽: 지점 리스트 -->
        <div class="location-selection-section">
          <div class="location-list">
            <div
              v-for="location in locations"
              :key="location.id"
              class="result-item"
              :class="{
                disabled: location.status === '점검중',
                selected: selectedLocation && selectedLocation.id === location.id,
              }"
              @click="selectLocationFromModal(location.id)"
            >
              <div class="result-info">
                <h4>{{ location.name }}</h4>
                <p>{{ location.address }}</p>
                <p class="locker-info">{{ location.lockers }}</p>
                <div class="location-meta">
                  <span class="distance">{{ location.distance }}</span>
                  <span
                    class="status"
                    :class="location.status === '운영중' ? 'operating' : 'maintenance'"
                  >
                    {{ location.status }}
                  </span>
                </div>
              </div>
              <div class="result-icon">📍</div>
            </div>
          </div>
        </div>

        <!-- 오른쪽: 지도 -->
        <div class="map-section-large">
          <div ref="modalMapEl" class="modal-map-large"></div>

          <div v-if="selectedLocation" class="location-card">
            <h4>{{ selectedLocation.name }}</h4>
            <p>{{ selectedLocation.address }}</p>
            <p>{{ selectedLocation.lockers }}</p>
            <div class="location-meta">
              <span class="distance">{{ selectedLocation.distance }}</span>
              <span
                class="status"
                :class="selectedLocation.status === '운영중' ? 'operating' : 'maintenance'"
              >
                {{ selectedLocation.status }}
              </span>
            </div>
            <button class="directions-btn">📍 길찾기</button>
          </div>
        </div>
      </div>
    </div>

    <!-- 푸터 -->
    <div class="footer">
      <button class="btn" @click="closeModal" :disabled="!selectedLocation">
        선택 완료
      </button>
    </div>
  </div>
</div>



  
</template>
<script setup>
import { ref, watch, onMounted, nextTick } from "vue";

const selectedLocationId = ref("");
const showModal = ref(false);
const selectedLocation = ref(null);
const modalMapEl = ref(null);

let map, marker, infoWindow, geocoder;

/* Kakao Map API key 존재 여부 */
const hasKakaoKey = Boolean(import.meta.env.VITE_KAKAO_MAP_APP_KEY);

/* Kakao Map 스크립트 로드 */
function loadKakaoMapScript() {
  return new Promise((resolve, reject) => {
    if (window.kakao && window.kakao.maps) return resolve();
    const key = import.meta.env.VITE_KAKAO_MAP_APP_KEY;
    const script = document.createElement("script");
    script.src = `https://dapi.kakao.com/v2/maps/sdk.js?autoload=false&appkey=${key}&libraries=services`;
    script.onload = () => {
      window.kakao.maps.load(resolve);
    };
    script.onerror = reject;
    document.head.appendChild(script);
  });
}

/* 지도 mount (모달 열릴 때 실행) */
async function mountMap() {
  if (!modalMapEl.value || !hasKakaoKey) return;

  await loadKakaoMapScript();

  geocoder = new window.kakao.maps.services.Geocoder();

  // 기본 중심 위치 (대구 시청 근처)
  const center = new window.kakao.maps.LatLng(35.8714, 128.6014);

  map = new window.kakao.maps.Map(modalMapEl.value, {
    center,
    level: 4,
  });

  marker = new window.kakao.maps.Marker({ position: center });
  marker.setMap(map);

  infoWindow = new window.kakao.maps.InfoWindow({ zIndex: 1 });

  // 드래그 후 중앙 마커 갱신
  window.kakao.maps.event.addListener(map, "dragend", () => {
    const pos = map.getCenter();
    marker.setPosition(pos);
  });

  if (selectedLocation.value) moveMapTo(selectedLocation.value);
}

/* 특정 위치로 지도 이동 */
function moveMapTo(location) {
  if (!geocoder || !map || !location) return;

  geocoder.addressSearch(location.address, (results, status) => {
    if (status === window.kakao.maps.services.Status.OK) {
      const { x, y } = results[0];
      const latlng = new window.kakao.maps.LatLng(y, x);
      map.setCenter(latlng);
      marker.setPosition(latlng);

      infoWindow.setContent(
        `<div style="padding:8px 10px;font-weight:600;">${location.name}</div>`
      );
      infoWindow.open(map, marker);
    }
  });
}

/* 모달 열릴 때 지도 mount */
watch(showModal, async (v) => {
  if (v) {
    await nextTick();
    mountMap();
  }
});

/*  지점 선택 시 지도 갱신 */
function selectLocationFromModal(locationId) {
  const location = locations.find((loc) => loc.id === locationId);
  if (!location || location.status === "점검중") return;

  selectedLocation.value = location;
  selectedLocationId.value = location.id;

  if (map) moveMapTo(location);
}

/* 모달 닫기 */
function closeModal() {
  showModal.value = false;
}

/* 모달 열기 */
function openMapModal() {
  showModal.value = true;
}
</script>

<style scoped>
.search-main .branch-search {
  --mint: #028587;
  --mint-weak: #f4fbfb;
  --ink: #111111;
  --muted: #647074;
  --ring: rgba(2, 133, 135, 0.26);
  --radius: 12px;

  background: #fff;

  padding: clamp(60px, 7vw, 100px) 0;
  @media (max-width: 768px) {
    padding: 100px 0 50px;
  }
}

.search-main .inner {
  width: 1320px;
  max-width: 100%;
  margin: 0 auto;
  display: flex;
  align-items: center;
  justify-content: center;
  gap: clamp(24px, 3vw, 40px);
  padding: 0 20px;
  @media (max-width: 390px) {
    gap: 0;
  }
}

/* 일러스트 자리 */
.search-main .illustration {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 250px;
  height: 250px;
  overflow: hidden;
  border-radius: 16px;
}

.search-main .illustration img {
  width: 100%;
  height: 100%;
  object-fit: contain;
}

/* 텍스트 영역 */
.search-main .content {
  flex: 1;
  max-width: 600px;
}

.search-main .title {
  font-weight: 800;
  font-size: clamp(24px, 3.2vw, 32px);
  letter-spacing: -0.02em;
  color: var(--ink);
  margin-bottom: 8px;
}

.search-main .subtitle {
  font-size: clamp(14px, 1.3vw, 15px);
  color: var(--muted);
  margin-bottom: 20px;
  @media (max-width: 390px){
    font-size: 12px;

  }

}

/* 검색 바 */
.search-main .searchbar {
  display: flex;
  align-items: center;
  justify-content: space-between;
  width: 100%;
  max-width: 560px;
  background: #fff;
  padding: 10px;
  border-radius: calc(var(--radius) + 4px);
  border: 1px solid #f0f3f3;
  box-shadow: 0 4px 24px rgba(0, 0, 0, 0.06);
  gap: 10px;
  @media screen and (max-width: 390px) {
    margin-top: 10px !important;
  }
}

.search-main .searchbar input,
.search-main .searchbar select {
  flex: 1;
  height: 46px;
  border: 1px solid #e7efef;
  border-radius: var(--radius);
  padding: 0 14px;
  font-size: 15px;
  outline: none;
  transition: border-color 0.15s ease, box-shadow 0.15s ease;
}

.location-select {
  background: white;
  cursor: pointer;
}

.location-select:focus {
  border-color: var(--mint);
  box-shadow: 0 0 0 4px var(--ring);
}

.search-main .searchbar input::placeholder {
  color: #9aa6a9;
}

.search-main .searchbar input:focus {
  border-color: var(--mint);
  box-shadow: 0 0 0 4px var(--ring);
}

.search-main .cta {
  flex-shrink: 0;
  height: 46px;
  padding: 0 18px;
  border: none;
  border-radius: var(--radius);
  background: var(--mint);
  color: #fff;
  font-weight: 700;
  font-size: 14px;
  cursor: pointer;
  transition: transform 0.06s ease, opacity 0.15s ease;
}
.search-main .cta:hover {
  opacity: 0.95;
}
.search-main .cta:active {
  transform: translateY(1px) scale(0.99);
}

/* 모달 스타일 */
.search-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;  padding-top: 5%;
}

.modal-content {
  width: 80vw;
  max-width: 1200px;
  height:60vh;
  background: white;
  border-radius: 12px;
  display: flex;
  flex-direction: column;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
}

.modal-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 16px 20px;
  border-bottom: 1px solid #eee;
}

.modal-header h3 {
  margin: 0;
  font-size: 18px;
  font-weight: 700;
}

.close-btn {
  background: none;
  border: none;
  font-size: 20px;
  cursor: pointer;
  padding: 4px;
}

.modal-body {
  flex: 1;
  display: flex;
  overflow: hidden;
}

/* 지점 선택 섹션 (왼쪽) */
.location-selection-section {
  width: 35%;
  border-right: 1px solid #eee;
  display: flex;
  flex-direction: column;
  padding: 16px;
}

.location-list {
  flex: 1;
  overflow-y: auto;
}

.result-item.selected {
  background-color: var(--mint-weak);
  border-left: 4px solid var(--mint);
}

/* 지도 섹션 (오른쪽) */
.map-section-large {
  width: 65%;
  position: relative;
}

.modal-map-large {
  width: 100%;
  height: 100%;
  min-height: 500px;
  border-radius: 8px;
  border: 1px solid #ddd;
  position: relative;
  overflow: hidden;
}

.search-section {
  width: 40%;
  border-right: 1px solid #eee;
  display: flex;
  flex-direction: column;
}

.search-input {
  padding: 16px;
  border-bottom: 1px solid #eee;
  display: flex;
  gap: 8px;
}

.search-input input {
  flex: 1;
  padding: 8px 12px;
  border: 1px solid #ddd;
  border-radius: 6px;
  font-size: 14px;
}

.result-item.disabled {
  opacity: 0.5;
  cursor: not-allowed;
}

.result-item.disabled:hover {
  background-color: transparent;
}

.location-meta {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 4px;
}

.distance {
  font-size: 11px;
  color: var(--mint);
  font-weight: 600;
}

.status {
  font-size: 10px;
  padding: 2px 6px;
  border-radius: 10px;
  font-weight: 600;
}

.status.operating {
  background: #e8f5e8;
  color: #2d5a2d;
}

.status.maintenance {
  background: #ffe8e8;
  color: #8b0000;
}

.search-input button {
  padding: 8px 12px;
  background: var(--mint);
  color: white;
  border: none;
  border-radius: 6px;
  cursor: pointer;
}

.search-results {
  flex: 1;
  overflow-y: auto;
}

.result-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 12px 16px;
  border-bottom: 1px solid #f0f0f0;
  cursor: pointer;
  transition: background-color 0.2s;
}

.result-item:hover {
  background-color: #f8f9fa;
}

.result-info h4 {
  margin: 0 0 4px 0;
  font-size: 14px;
  font-weight: 600;
}

.result-info p {
  margin: 0;
  font-size: 12px;
  color: var(--muted);
}

.result-icon {
  font-size: 16px;
}

.no-results {
  padding: 20px;
  text-align: center;
  color: var(--muted);
}

.no-results p {
  margin: 0;
  font-size: 14px;
}

.map-section {
  flex: 1;
  position: relative;
}

.modal-map {
  width: 100%;
  height: 100%;
  min-height: 300px;
  /* background: #f8f9fa; */
  border-radius: 8px;
  border: 1px solid #ddd;
  position: relative;
  overflow: hidden;
}

.location-card {
  position: absolute;
  bottom: 16px;
  right: 16px;
  background: white;
  padding: 12px;
  border-radius: 8px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  max-width: 200px;
}

.location-card h4 {
  margin: 0 0 4px 0;
  font-size: 14px;
  font-weight: 600;
}

.location-card p {
  margin: 0 0 4px 0;
  font-size: 12px;
  color: var(--muted);
}

.directions-btn {
  background: var(--mint);
  color: white;
  border: none;
  padding: 6px 12px;
  border-radius: 4px;
  font-size: 12px;
  cursor: pointer;
  margin-top: 8px;
}

/* 접근성용 라벨 숨김 */
.search-main .a11y {
  position: absolute !important;
  width: 1px;
  height: 1px;
  margin: -1px;
  padding: 0;
  border: 0;
  clip: rect(0 0 0 0);
  overflow: hidden;
}

/* ✅ 반응형 수정 버전 */
@media (max-width: 1024px) {
  .modal-content {
     width: 70vw;
    height: 50vh;
  }

  .location-selection-section {
    width: 40%;
  }

  .map-section-large {
    width: 60%;
  }
}

@media (max-width: 768px) {
  .search-main .inner {
    flex-direction: column;
    text-align: center;
  }

  .search-main .illustration {
    width: 200px;
    height: 250px;
  }

  .search-main .illustration img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }

  .search-main .content {
    max-width: 100%;
  }

  /* 💡 인풋 크기 확대 */
  .search-main .searchbar {
    flex-direction: column;
    max-width: 90%; /* 화면의 90%로 확장 */
    margin: 0 auto;
    padding: 12px;
  }

  .search-main .searchbar input,
  .search-main .searchbar select {
    width: 100%;
    min-width: 260px;
    font-size: 15px;
    padding: 10px 0;
  }

  .search-main .cta {
    width: 100%;
    height: 46px;
    margin-top: 8px;
  }

  /* 모달 모바일 스타일 */
  .modal-content {
    width: 95vw;
    height: 90vh;
  }

  .modal-body {
    flex-direction: column;
  }

  .location-selection-section {
    width: 100%;
    height: 45%;
    border-right: none;
    border-bottom: 1px solid #eee;
    padding: 12px;
  }

  .map-section-large {
    width: 100%;
    height: 55%;
  }

  .modal-map-large {
    min-height: 300px;
  }

  .location-card {
    position: relative;
    bottom: auto;
    right: auto;
    margin: 12px;
    max-width: none;
  }

  .result-item {
    padding: 10px 12px;
  }

  .result-info h4 {
    font-size: 13px;
  }

  .result-info p {
    font-size: 11px;
  }
}

@media (max-width: 480px) {
  .search-main .illustration {
    width: 180px;
    height: 250px;
  }

  .search-main .illustration img {
    width: 100%;
    height: 100%;
    object-fit: contain;
  }

  .modal-content {
    width: 90vw;
    height: 50vh;
  }

  .location-selection-section {
    /* height: 40%; */

    padding: 20px 0;
  }

  .map-section-large {
    height: 60%;
  }

  .modal-map-large {
    min-height: 250px;
  }

  .result-item {
    padding: 8px 10px;
  }

  .result-info h4 {
    font-size: 12px;
  }

  .result-info p {
    font-size: 10px;
  }

  .location-meta {
    flex-direction: column;
    align-items: flex-start;
    gap: 2px;
  }
}
</style>
