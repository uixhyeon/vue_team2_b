<template>
  <div class="search-main">
    <section class="branch-search" role="search" aria-label="마타주 지점 찾기">
      <div class="inner">
        <!-- 이미지 표시 영역 -->
        <figure class="illustration">
          <img src="/images/search.png" alt="지점 찾기" />
        </figure>

        <div class="content">
          <h1 class="title">마타주 지점 찾기</h1>
          <p class="subtitle">근처에 있는 마타주 보관소와 무인함 위치를 한눈에 확인할 수 있습니다.</p>

          <form class="searchbar" @submit.prevent="openMapModal">
            <label class="a11y" for="branchSelect">지점 선택</label>

            <select
              id="branchSelect"
              v-model="selectedLocationId"
              class="location-select"
              @change="selectLocationFromDropdown"
            >
              <option value="">지점을 선택해주세요</option>
              <!-- region 안의 branch 반복 -->
              <optgroup v-for="region in locations" :key="region.region" :label="region.region">
                <option
                  v-for="branch in region.branches"
                  :key="branch.id"
                  :value="branch.id"
                  :disabled="branch.status === '점검중'"
                >
                  {{ region.region }} - {{ branch.name }}
                  {{ branch.status === "점검중" ? " (점검중)" : "" }}
                </option>
              </optgroup>
            </select>

            <button class="cta" type="submit" :disabled="!selectedLocationId">지점 확인하기</button>
          </form>
          <BranchSelectModal
            :open="showModal"
            :locations="locations"
            @close="closeModal"
            @selected="handleBranchSelect"
          />
        </div>
      </div>
    </section>
  </div>
</template>

<script setup>
import BranchSelectModal from "@/components/reserv/BranchSelectModal.vue";
import { ref, onMounted, nextTick } from "vue";

// 반응형 데이터
const selectedLocationId = ref("");
const showModal = ref(false);
const selectedLocation = ref(null);
const modalMapEl = ref(null);
// ===== 지점 선택 핸들러 =====
function handleBranchSelect(location) {
  form.value.address = location.name; // BranchSelectModal에서 전달한 값
  showModal.value = false;
}
// 📍 지역별 지점 리스트
const locations = [
  {
    region: "부산 광안리",
    branches: [
      {
        id: 1,
        name: "광안리 해변점",
        address: "부산광역시 수영구 광안해변로 203",
        lockers: "S~XL 보유",
        status: "운영중",
      },
      {
        id: 2,
        name: "광안시장점",
        address: "부산광역시 수영구 남천동로 12-1", // 📍 실제 존재 주소
        lockers: "S~L 보유",
        status: "운영중",
      },
      {
        id: 3,
        name: "광안역점",
        address: "부산광역시 수영구 광안로 45",
        lockers: "M~XXL 보유",
        status: "점검중",
      },
    ],
  },
  {
    region: "강릉시",
    branches: [
      {
        id: 4,
        name: "강릉역점",
        address: "강원특별자치도 강릉시 용지로 123", // 📍 '강원특별자치도'로 변경됨
        lockers: "S~L 보유",
        status: "운영중",
      },
      {
        id: 5,
        name: "경포해변점",
        address: "강원특별자치도 강릉시 창해로 240-3", // 📍 경포해수욕장 인근 실제 도로명
        lockers: "S~XL 보유",
        status: "운영중",
      },
    ],
  },
  {
    region: "속초",
    branches: [
      {
        id: 6,
        name: "속초중앙시장점",
        address: "강원특별자치도 속초시 중앙로 147", // 📍 중앙시장 중심 좌표
        lockers: "S~L 보유",
        status: "운영중",
      },
      {
        id: 7,
        name: "속초해수욕장점",
        address: "강원특별자치도 속초시 해오름로 190", // 📍 실제 해수욕장 중심 위치
        lockers: "M~XL 보유",
        status: "점검중",
      },
    ],
  },
  {
    region: "전주",
    branches: [
      {
        id: 8,
        name: "전주한옥마을점",
        address: "전라북도 전주시 완산구 기린대로 99", // 📍 실제 한옥마을 입구 인근
        lockers: "S~XL 보유",
        status: "운영중",
      },
    ],
  },
  {
    region: "제주도",
    branches: [
      {
        id: 9,
        name: "제주시청점",
        address: "제주특별자치도 제주시 관덕로 9", // 📍 제주시청 맞은편
        lockers: "S~XL 보유",
        status: "운영중",
      },
      {
        id: 10,
        name: "서귀포점",
        address: "제주특별자치도 서귀포시 중문관광로 72", // 📍 중문관광단지 내 위치
        lockers: "M~XXL 보유",
        status: "운영중",
      },
    ],
  },
  {
    region: "오사카",
    branches: [
      {
        id: 11,
        name: "난바역점",
        address: "Namba Station, Osaka, Japan",
        lockers: "S~L 보유",
        status: "운영중",
      },
      {
        id: 12,
        name: "우메다점",
        address: "2-14-7 Sonezaki, Kita Ward, Osaka, Japan",
        lockers: "M~XXL 보유",
        status: "운영중",
      },
    ],
  },
];

// 모달에 전달할 지역 그룹 형태로 변환
const locationGroups = [
  {
    region: "대구",
    branches: locations,
  },
  {
    region: "속초",
    branches: locations,
  },
];

// 모달에서 지점 선택 완료 처리
function handleLocationSelected(locationData) {
  const location = locations.find((loc) => loc.name === locationData.name);
  if (location) {
    selectedLocation.value = location;
    selectedLocationId.value = location.id.toString();
  }
  showModal.value = false;
}

// 지도 클릭시 모달 열기
function openMapModal() {
  showModal.value = true;
  nextTick(() => {
    if (modalMapEl.value) {
      if (window.kakao && window.kakao.maps) {
        createRealMap();
      } else {
        createDefaultMap();
      }
    }
  });
}

// 드롭다운에서 지점 선택
function selectLocationFromDropdown() {
  if (!selectedLocationId.value) return;

  let found = null;
  for (const region of locations) {
    const branch = region.branches.find((b) => b.id === parseInt(selectedLocationId.value));
    if (branch) {
      found = { region: region.region, ...branch };
      break;
    }
  }

  if (found) {
    selectedLocation.value = found;
  }
}

// 모달에서 지점 선택
function selectLocationFromModal(locationId) {
  if (!locationId) return;

  const location = locations.find((loc) => loc.id === locationId);
  if (location && location.status !== "점검중") {
    selectedLocation.value = location;

    // 모달 지도 업데이트
    nextTick(() => {
      if (modalMapEl.value) {
        updateModalMap(location);
      }
    });
  }
}

// 모달 내 검색
function performSearch() {
  const query = modalSearchQuery.value.toLowerCase();

  if (query.trim() === "") {
    // 검색어가 없으면 모든 결과 표시
    searchResults.value = sampleLocations;
  } else {
    // 검색어가 있으면 필터링
    searchResults.value = sampleLocations.filter(
      (location) => location.name.toLowerCase().includes(query) || location.address.toLowerCase().includes(query)
    );
  }

  // 검색 결과가 있으면 첫 번째 결과를 선택
  if (searchResults.value.length > 0) {
    selectLocation(searchResults.value[0]);
  }
}

// 위치 선택
function selectLocation(location) {
  selectedLocation.value = location;
  // 모달 지도 업데이트
  nextTick(() => {
    if (modalMapEl.value) {
      updateModalMap(location);
    }
  });
}

// 모달 닫기
function closeModal() {
  showModal.value = false;
  // 선택한 위치가 있으면 드롭다운 업데이트
  if (selectedLocation.value) {
    selectedLocationId.value = selectedLocation.value.id;
  }
}

// 카카오맵 API 로드 및 초기화
let kakaoMap = null;
let kakaoMarker = null;
let isKakaoMapLoading = false;
let kakaoMapLoadAttempted = false;

async function loadKakaoMapAPI() {
  return new Promise((resolve, reject) => {
    // 이미 로드되었으면 바로 resolve
    if (window.kakao && window.kakao.maps) {
      resolve();
      return;
    }

    // 이미 로딩 중이면 대기
    if (isKakaoMapLoading) {
      const checkInterval = setInterval(() => {
        if (window.kakao && window.kakao.maps) {
          clearInterval(checkInterval);
          resolve();
        }
      }, 100);
      return;
    }

    // 이미 시도했고 실패했으면 바로 reject
    if (kakaoMapLoadAttempted) {
      reject(new Error("카카오맵 API 로드 실패"));
      return;
    }

    isKakaoMapLoading = true;
    kakaoMapLoadAttempted = true;

    const script = document.createElement("script");
    script.src = "//dapi.kakao.com/v2/maps/sdk.js?autoload=false";
    script.onload = () => {
      window.kakao.maps.load(() => {
        isKakaoMapLoading = false;
        resolve();
      });
    };
    script.onerror = (error) => {
      isKakaoMapLoading = false;
      // 콘솔 경고 제거 - 정상적인 상황이므로 조용히 처리
      reject(error);
    };
    document.head.appendChild(script);
  });
}

// 실제 카카오맵 생성
function createRealMap() {
  if (!modalMapEl.value) return;

  // 카카오맵이 로드되었는지 확인
  if (!window.kakao || !window.kakao.maps) {
    createDefaultMap();
    return;
  }

  // 카카오맵 로드 완료 후 실행
  window.kakao.maps.load(() => {
    // 기본 위치 (대구 중구)
    const defaultPosition = new window.kakao.maps.LatLng(35.8714, 128.6014);

    // 지도 생성
    const mapOption = {
      center: defaultPosition,
      level: 3,
    };

    kakaoMap = new window.kakao.maps.Map(modalMapEl.value, mapOption);

    // 마커 이미지 설정
    const markerImageSrc = "https://t1.daumcdn.net/localimg/localimages/07/mapapidoc/marker_red.png";
    const markerImageSize = new window.kakao.maps.Size(40, 40);
    const markerImage = new window.kakao.maps.MarkerImage(markerImageSrc, markerImageSize);

    // 마커 생성
    kakaoMarker = new window.kakao.maps.Marker({
      position: defaultPosition,
      image: markerImage,
    });
    kakaoMarker.setMap(kakaoMap);

    console.log("카카오맵 생성 완료");
  });
}

// 기본 지도 생성
function createDefaultMap() {
  if (!modalMapEl.value) return;

  // 강제로 지도 내용 생성
  modalMapEl.value.innerHTML = `
    <div style="width: 100%; height: 100%; background: #e8f4f8; border-radius: 8px; display: flex; align-items: center; justify-content: center; position: relative; border: 2px solid #028587;">
      <div style="position: absolute; top: 10px; left: 10px; background: white; padding: 8px; border-radius: 4px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); font-size: 12px;">
        📍 지점을 선택해주세요
      </div>
      <div style="font-size: 48px; color: #028587;">🗺️</div>
    </div>
  `;
}

// 모달 지도 업데이트
function updateModalMap(location) {
  if (!modalMapEl.value) return;

  // 카카오맵이 있으면 실제 지도 업데이트
  if (kakaoMap && kakaoMarker && window.kakao && window.kakao.maps) {
    const position = new window.kakao.maps.LatLng(location.lat, location.lng);
    kakaoMap.setCenter(position);
    kakaoMarker.setPosition(position);

    // 인포윈도우 표시
    const infowindow = new window.kakao.maps.InfoWindow({
      content: `<div style="padding: 10px; font-weight: bold; font-size: 14px;">${location.name}</div>`,
    });
    infowindow.open(kakaoMap, kakaoMarker);
  } else {
    // 기본 지도 업데이트
    modalMapEl.value.innerHTML = `
      <div style="width: 100%; height: 100%; background: #e8f4f8; border-radius: 8px; display: flex; align-items: center; justify-content: center; position: relative; border: 2px solid #028587;">
        <div style="position: absolute; top: 10px; left: 10px; background: white; padding: 8px; border-radius: 4px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); font-size: 12px;">
          📍 ${location.name}
        </div>
        <div style="font-size: 48px; color: #028587;">🗺️</div>
      </div>
    `;
  }
}

onMounted(() => {
  // 초기화
});
</script>

<style scoped>
.search-main .branch-search {
  --mint: #3e9c9b;
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
  font-size: clamp(24px, 3.2vw, 35px);
  letter-spacing: -0.02em;
  color: var(--ink);
  margin-bottom: 8px;
}

.search-main .subtitle {
  font-size: clamp(14px, 1.3vw, 17px);
  color: var(--muted);
  margin-bottom: 20px;
  @media (max-width: 390px) {
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
  color: #000;
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
  background: #fa5382;
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
  z-index: 1000;
  padding-top: 5%;
}

.modal-content {
  width: 80vw;
  max-width: 1200px;
  height: 60vh;
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
