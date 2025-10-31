<template>
  <div class="wrap">
    <Stepper :current-step="1" />
    <div class="background inner">
      <div class="container">
        <!-- 왼쪽 입력 카드 -->
        <div class="left">
          <!-- 사물함 예약 -->
          <div class="form_card" :class="{ open: openSection === 'locker' }">
            <div class="card_header" @click="toggleSection('locker')">
              <h3>사물함 예약*</h3>
         <i v-if="lockerComplete" class="fa-solid fa-check" style="color:#53b4a1"></i>

            </div>

            <transition name="fade">
              <div v-show="openSection === 'locker'" class="card_content" @click.stop>
                <!-- 이름 -->
                <div class="form_group">
                  <label>성함*</label>
                  <input type="text" placeholder="성함을 입력해주세요" v-model="form.name" />
                  <p class="label">문자를 입력하세요</p>
                </div>

                <!-- 전화번호 -->
                <div class="form_group">
                  <label>휴대폰 번호*</label>
                  <input type="text" placeholder="01012345678 " v-model="form.phone" />
                  <p class="label">숫자를 입력해주세요 (- 제외)</p>
                </div>

                <!-- 사이즈 -->
                <div class="form_group">
                  <label>사물함 사이즈*</label>
                  <select v-model="form.size">
                    <option value="">사이즈를 선택해 주세요</option>
                    <option>S</option>
                    <option>M</option>
                    <option>L</option>
                    <option>XL</option>
                    <option>XXL</option>
                  </select>
                  <p class="label">상세길이는 이용안내를 확인해주세요</p>
                </div>

                <!--주소 선택 -->
                <div class="form_group">
                  <label>대여 장소*</label>
                  <div class="addr-input">
                    <input type="text" placeholder="지번 및 주소를 입력해주세요" v-model="form.address" readonly />
                    <button type="button" class="mini-btn" @click="showModal = true">지점 선택</button>
                  </div>
                  <p class="label">대여장소를 선택해 주세요</p>

                  <!--지점선택 모달 -->
                  <BranchSelectModal
                    :open="showModal"
                    :locations="locations"
                    @close="showModal = false"
                    @selected="handleSelect"
                  />
                </div>

                <!-- 예약 기간 -->
                <div class="form_group">
                  <label>예약 기간*</label>
                  <VueDatePicker
                    v-model="form.dateRange"
                    range
                    locale="ko"
                    placeholder="기간을 선택하세요"
                    :enable-time-picker="false"
                    format="yyyy-MM-dd"
                  />
                  <p class="label">시작일과 종료일을 모두 선택하세요</p>
                </div>
              </div>
            </transition>
          </div>

          <!-- 짐 가져오기 -->
          <div class="form_card" :class="{ open: openSection === 'arrival' }">
            <div class="card_header" @click="toggleSection('arrival')">
              <h3>짐 가져오기</h3>
      
           <i v-if="arrivalComplete" class="fa-solid fa-check" style="color:#53b4a1"></i>

            </div>

            <transition name="fade">
              <div v-show="openSection === 'arrival'" class="card_content" @click.stop>
                <div class="form_group">
                  <label>주소*</label>
                  <div class="addr-input">
                    <input
                      type="text"
                      placeholder="지번 및 주소를 검색해주세요"
                      v-model="form.pickupAddress"
                      readonly
                      @click="openPickupAddr = true"
                    />
                    <button type="button" class="mini-btn" @click="openPickupAddr = true">주소 검색</button>
                  </div>
                  <p class="label">보내실 주소를 선택해 주세요</p>

                  <div class="blank16">{{ form.pickupAddress || "주소를 선택해주세요" }}</div>

                  <input type="text" placeholder="상세주소를 입력해주세요" v-model="form.pickupAddressDetail" />
                  <p class="label">상세주소를 입력해 주세요</p>
                </div>

                <div class="form_group">
                  <label>배송 지정일*</label>
               <VueDatePicker
    v-model="form.pickupDate"
    locale="ko"
    :enable-time-picker="false"
    format="yyyy-MM-dd"
    placeholder="날짜를 선택하세요"
  />
  <p class="label">보내실 날짜를 선택하세요</p>
                </div>
              </div>
            </transition>
          </div>

          <!-- 집으로 보내기 -->
          <div class="form_card" :class="{ open: openSection === 'luggage' }">
            <div class="card_header" @click="toggleSection('luggage')">
              <h3>집으로 보내기</h3>
         
            <i v-if="luggageComplete" class="fa-solid fa-check" style="color:#53b4a1"></i>
            </div>

            <transition name="fade">
              <div v-show="openSection === 'luggage'" class="card_content" @click.stop>
                <div class="form_group">
                  <label>주소*</label>
                  <div class="addr-input">
                    <input
                      type="text"
                      placeholder="지번 및 주소를 입력해주세요"
                      v-model="form.homeAddress"
                      readonly
                      @click="openHomeAddr = true"
                    />
                    <button type="button" class="mini-btn" @click="openHomeAddr = true">주소 검색</button>
                  </div>
                  <p class="label">받으실 주소를 선택해 주세요</p>
                  <div class="blank16">{{ form.homeAddress || "주소를 선택해주세요" }}</div>
                  <input type="text" placeholder="상세주소를 입력해주세요" v-model="form.homeAddressDetail" />
                  <p class="label">상세주소를 입력해 주세요</p>
                </div>

                <div class="form_group">
                  <label>배송일*</label>
                   <VueDatePicker
  v-model="form.deliveryDate"
  locale="ko"
  :enable-time-picker="false"
  format="yyyy-MM-dd"
  placeholder="날짜를 선택하세요"
/>
                
                  <p class="label">배송일을 지정해 주세요</p>
                </div>
              </div>
            </transition>
          </div>
        </div>

        <!-- 오른쪽 요약 -->
        <div class="right">
          <div class="summary_card">
            <h2 class="card_title">선택 상품 요약</h2>

            <!--입력 전 안내문 -->
            <div v-if="!hasInput" class="summary-guide">원하시는 상품을 선택해주세요 🧳</div>

            <!--입력 후 요약 테이블 -->
            <table v-else>
              <colgroup>
                <col style="width: 45%" />
                <col style="width: 55%" />
              </colgroup>
              <tbody>
                <!--사물함 예약 -->
               <template
  v-if="lockerComplete || form.name || form.phone || form.size || form.address || form.dateRange"
>

                  <tr class="section-title">
                    <td colspan="2" class="s-title">사물함 예약</td>
                  </tr>
                  <tr v-if="form.name">
                    <td>성함</td>
                    <td>{{ form.name }}</td>
                  </tr>
                  <tr v-if="form.phone">
                    <td>휴대폰</td>
                    <td>{{ form.phone }}</td>
                  </tr>
                  <tr v-if="form.size">
                    <td>사물함 사이즈</td>
                    <td>{{ form.size }}</td>
                  </tr>
                  <tr v-if="form.address">
                    <td>대여 장소</td>
                    <td>{{ form.address }}</td>
                  </tr>
                  <tr v-if="form.dateRange && form.dateRange[0] && form.dateRange[1]">
                    <td>예약 기간</td>
                    <td>{{ formatDate(form.dateRange[0]) }} ~<br/> {{ formatDate(form.dateRange[1]) }}</td>
                  </tr>
                </template>

                <!--짐 가져오기 -->
                <template v-if="form.pickupAddress || form.pickupAddressDetail || form.pickupDate">
                  <tr class="section-title">
                    <td colspan="2" class="s-title">짐 가져오기</td>
                  </tr>
                  <tr v-if="form.pickupAddress">
                    <td>픽업 주소</td>
                    <td>{{ form.pickupAddress }}</td>
                  </tr>
                  <tr v-if="form.pickupAddressDetail">
                    <td>상세주소</td>
                    <td>{{ form.pickupAddressDetail }}</td>
                  </tr>
                  <tr v-if="form.pickupDate">
                    <td>픽업일</td>
  <td>{{ formatDate(form.pickupDate) }}</td>
                  </tr>
                </template>

                <!-- 집으로 보내기 -->
                <template v-if="form.homeAddress || form.homeAddressDetail || form.deliveryDate">
                  <tr class="section-title">
                    <td colspan="2" class="s-title">집으로 보내기</td>
                  </tr>
                  <tr v-if="form.homeAddress">
                    <td>배송 주소</td>
                    <td>{{ form.homeAddress }}</td>
                  </tr>
                  <tr v-if="form.homeAddressDetail">
                    <td>상세주소</td>
                    <td>{{ form.homeAddressDetail }}</td>
                  </tr>
                  <tr v-if="form.deliveryDate">
                    <td>배송일</td>
                    <td>{{ formatDate(form.deliveryDate) }}</td>
                  </tr>
                </template>

                <!-- 총 결제금액 -->
                <tr v-if="totalPrice > 0" class="total">
                  <td class="s-title">총 결제금액</td>
                  <td>
                    <strong>{{ formatKrw(totalPrice) }}</strong>
                  </td>
                </tr>
              </tbody>
            </table>
          </div>

          <button class="submit_btn" @click="goTopayment">입력 완료</button>
        </div>
      </div>
    </div>

    <!-- 모달들 -->
    <HySearch
      v-if="openHySearch"
      :open="openHySearch"
      :location="selectedLocation"
      @close="openHySearch = false"
      @select="
        (location) => {
          form.address = location.address;
          openHySearch = false;
        }
      "
    />

    <!-- 짐 가져오기 주소 -->
    <AddressPicker v-model="form.pickupAddress" :open="openPickupAddr" @close="openPickupAddr = false" />

    <!-- 집으로 보내기 주소 -->
    <AddressPicker v-model="form.homeAddress" :open="openHomeAddr" @close="openHomeAddr = false" />
  </div>
</template>

<script setup>
import { useRouter } from "vue-router";
import { ref, computed } from "vue";
import Stepper from "@/components/reserv/Stepper.vue";
import AddressPicker from "@/components/reserv/AddressPicker.vue";
import BranchSelectModal from "@/components/reserv/BranchSelectModal.vue";
import { watch, onMounted } from "vue";

// 페이지넘어감
const router = useRouter();
// 기존
// router.push("/reservation2");

const goTopayment = () => {
  alert("입력완료");
  const formData = JSON.stringify(form.value);
  router.push({
    path: "/reservation2",
    query: {
      form: formData,
      totalPrice: totalPrice.value,
    },
  });
};
//  DatePicker import
import VueDatePicker from "@vuepic/vue-datepicker";
import "@vuepic/vue-datepicker/dist/main.css";

// //HySearch
// import HySearch from "@/views/booking/HySearch.vue";

// 하나라도 입력이 있으면 안내문 숨기기
const hasInput = computed(() => {
  const f = form.value;
  return (
    f.name ||
    f.phone ||
    f.size ||
    f.address ||
    (f.dateRange && f.dateRange[0]) ||
    f.pickupAddress ||
    f.pickupAddressDetail ||
    f.pickupDate ||
    f.homeAddress ||
    f.homeAddressDetail ||
    f.deliveryDate
  );
});

//=======selectedService=============
const selectedService = computed(() => {
  switch (openSection.value) {
    case "locker":
      return "사물함 예약";
    case "arrival":
      return "짐 가져오기";
    case "luggage":
      return "집으로 보내기";
    default:
      return "";
  }
});

// ===============열닫토글=================
const openSection = ref("locker");
const toggleSection = (name) => {
  openSection.value = openSection.value === name ? null : name;
};

const showModal = ref(false);

// 지점 데이터 (실제로는 API에서 가져올 데이터)
const locations = [
  {
    id: 1,
    name: "칠성시장점",
    address: "대구광역시 중구 동성로2가 189-1",
    lockers: "잔여 사물함 S: 2개 XL: 2개",
    lat: 35.8714,
    lng: 128.6014,
    status: "운영중",
    distance: "0.2km",
  },
  {
    id: 2,
    name: "동성로점",
    address: "대구광역시 중구 동성로 123",
    lockers: "잔여 사물함 S: 5개 XL: 1개",
    lat: 35.87,
    lng: 128.6,
    status: "운영중",
    distance: "0.5km",
  },
  {
    id: 3,
    name: "중앙로점",
    address: "대구광역시 중구 중앙대로 456",
    lockers: "잔여 사물함 S: 3개 XL: 3개",
    lat: 35.872,
    lng: 128.602,
    status: "운영중",
    distance: "0.8km",
  },
  {
    id: 4,
    name: "서문시장점",
    address: "대구광역시 중구 대신동 115-1",
    lockers: "잔여 사물함 S: 4개 XL: 2개",
    lat: 35.8698,
    lng: 128.5856,
    status: "운영중",
    distance: "1.2km",
  },
  {
    id: 5,
    name: "반월당점",
    address: "대구광역시 중구 동성로1가 88-1",
    lockers: "잔여 사물함 S: 1개 XL: 4개",
    lat: 35.8667,
    lng: 128.5956,
    status: "운영중",
    distance: "1.5km",
  },
  {
    id: 6,
    name: "대구역점",
    address: "대구광역시 동구 동부로 149",
    lockers: "잔여 사물함 S: 6개 XL: 3개",
    lat: 35.8759,
    lng: 128.6285,
    status: "운영중",
    distance: "2.1km",
  },
  {
    id: 7,
    name: "수성못점",
    address: "대구광역시 수성구 두산동 100",
    lockers: "잔여 사물함 S: 3개 XL: 2개",
    lat: 35.8251,
    lng: 128.6304,
    status: "운영중",
    distance: "3.2km",
  },
  {
    id: 8,
    name: "동대구역점",
    address: "대구광역시 동구 동부로 149",
    lockers: "잔여 사물함 S: 2개 XL: 1개",
    lat: 35.8779,
    lng: 128.6285,
    status: "점검중",
    distance: "2.3km",
  },
];

function handleSelect(location) {
  console.log("선택된 지점:", location);
  form.value.address = `${location.name}`;
  showModal.value = false;
}

const openAddr = ref(false);

const openPickupAddr = ref(false); // 짐 가져오기용 모달
const openHomeAddr = ref(false); // 집으로 보내기용 모달

const form = ref({
  name: "",
  phone: "",
  size: "",
  address: "",
  dateRange: null, // ✅ 추가 — 기간 저장
  pickupDate: "",
  pickupAddress: "",
  pickupAddressDetail: "",
  pickupDate: "",
  homeAddress: "",
  homeAddressDetail: "",
  deliveryDate: "",
});
// 체크표시

const lockerComplete = ref(false);
const arrivalComplete = ref(false);
const luggageComplete = ref(false);

watch(
  form,
  (f) => {
    lockerComplete.value =
      f.name.trim() &&
      f.phone.trim() &&
      f.size.trim() &&
      f.address.trim() &&
      f.dateRange &&
      f.dateRange[0] &&
      f.dateRange[1];

    arrivalComplete.value =
      f.pickupAddress.trim() &&
      f.pickupAddressDetail.trim() &&
      f.pickupDate;

    luggageComplete.value =
      f.homeAddress.trim() &&
      f.homeAddressDetail.trim() &&
      f.deliveryDate;
  },
  { deep: true }
);


// ================코드..==================
// ✅ 사이즈별 요금표
const prices = {
  S: { locker: 5000, delivery: 4000 },
  M: { locker: 8000, delivery: 6000 },
  L: { locker: 15000, delivery: 14000 },
  XL: { locker: 20000, delivery: 22000 },
  XXL: { locker: 28000, delivery: 32000 },
};

// ✅ 선택된 사이즈별 카드 요금 계산 (안전 버전)
const lockerPrice = computed(() => {
  const size = form.value.size || ""; // 비어있을 때도 "" 처리
  return prices[size]?.locker ?? 0;
});
const arrivalPrice = computed(() => {
  const size = form.value.size || "";
  return prices[size]?.delivery ?? 0;
});
const luggagePrice = computed(() => {
  const size = form.value.size || "";
  return prices[size]?.delivery ?? 0;
});

// ✅ 합계 (숫자 아닌 건 자동 제외, 테스트용: 조건 제외)
const totalPrice = computed(() => {
  const f = form.value;
  let total = 0;

if (lockerComplete.value) total += lockerPrice.value;
if (arrivalComplete.value) total += arrivalPrice.value;
if (luggageComplete.value) total += luggagePrice.value;

  return total;
});

const formatKrw = (value) => {
  const num = Number(value);
  if (isNaN(num)) return "₩0";
  return new Intl.NumberFormat("ko-KR", {
    style: "currency",
    currency: "KRW",
  }).format(num);
};
// 지점 지정

const handleSubmit = () => {
  alert(`"${selectedService.value}" 예약 정보가 저장되었습니다.`);
};

// 날짜 포맷 함수 추가
const formatDate = (date) => {
  if (!date) return "";
  const d = new Date(date);
  const year = d.getFullYear();
  const month = String(d.getMonth() + 1).padStart(2, "0");
  const day = String(d.getDate()).padStart(2, "0");
  return `${year}-${month}-${day}`;
};
// =========날짜~조정==

// ========================
defineProps({
  open: Boolean,
  locations: Array,
});
defineEmits(["close", "selected"]);
</script>

<style scoped lang="scss">
@use "/src/assets/style/variables" as *;

/* =========================================================
 ✅ 1. 기본 레이아웃
========================================================= */
.wrap {
  background: #f5f7f7;
  padding: 40px 0 110px 0;
}

.inner {
  max-width: 1120px;
  margin: 0 auto;
  padding: 0 20px;
  box-sizing: border-box;

  .container {
    display: grid;
    grid-template-columns: 3fr 2fr;
    gap: 2rem;
    align-items: flex-start;
    width: 100%;
    max-width: 1120px;
    margin: 0 auto;
  }

  .left {
    display: flex;
    flex-direction: column;
    gap: 1.3rem;
  }

  .right {
    position: static;
    align-self: flex-start;
    display: flex;
    flex-direction: column;
    gap: 1.3rem;
    align-items: center;
  }
}

.blank16 {
  height: 4px;
  color: transparent;
}

/* =========================================================
 ✅ 2. 카드 공통 스타일
========================================================= */
.form_card,
.summary_card {
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  border: 1px solid transparent;
  position: relative;
  transition: all 0.3s ease;
  padding: 30px 40px;
  box-sizing: border-box;
  border: 1px solid #f0f0f0; /* ✅ 테두리 추가 */
  font-size: $text-sm;
  color: #444;

  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 12px;
    background: $color_main;
    border-top-left-radius: 10px;
    border-top-right-radius: 10px;
  }
}

/* =========================================================
 ✅ 3. 왼쪽 입력 카드
========================================================= */
.form_card {
  padding: 30px 40px; /* ✅ Reservation2와 동일 */

  .card_header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    cursor: pointer;

    h3 {
      font-size: $text-md;
      font-weight: 600;
      margin: 0;
    }

    img {
      width: 20px;
      transition: transform 0.3s ease;
    }

    .rotate {
      transform: rotate(180deg);
    }
  }

  /* 입력 폼 */
  .form_group {
    margin-bottom: 20px;

    label {
      padding-left: 3px;
      display: block;
      font-size: $label-md;
      color: #555;
      margin-top: 20px;
      margin-bottom: 6px;
      font-weight: 500;
    }
select {
  color: #aaa !important ; /* 원하는 글자색 */

}
    input,
    select {
      width: 100%;
      border: none;
      border-bottom: 1px solid #e7e7e7;
      background: transparent;
      font-size: $label-md;
      padding: 10px 10px;
      color: #333;
      transition: border-color 0.2s ease;

      &:focus {
        border-bottom: 1px solid $color_main_light;
        outline: none;
      }

      &::placeholder {
        color: #aaa;
      }
    }

    .label {
      font-size: $label-sm;
      color: #888;
      margin-top: 4px;
      padding-left: 3px;
    }
  }

  /* 주소 입력 */
  .addr-input {
    display: flex;
    gap: 8px;
    align-items: center;

    .mini-btn {
      padding: 8px 10px;
      border-radius: 6px;
      background:$color_main;
      color: #fff;
      border: none;
      cursor: pointer;
      font-size: $label-sm;

      &:hover {
        background: $color_main_deep;
      }
    }
  }
}

/* =========================================================
 ✅ 4. 오른쪽 요약 카드
========================================================= */
.summary_card {
  width: 100%;
  border-radius: 8px;
  padding: 30px 40px;

  .card_title {
    font-size: $text-md;
    font-weight: 600;
    color: #222;
    margin-bottom: 20px;
  }

  table {
    width: 100%;
    border-collapse: collapse;
    font-size: $text-sm;
    color: #444;
    margin-bottom: 24px;

    tr {
      border-bottom: 1px solid #e7e7e7;

      &:last-child {
        border-bottom: none;
      }

      &.total td {
        font-weight: 600;
        color: #111;

        &:last-child {
          font-size: $text-sm;
          color: $color_main_light;
        }
      }
    }

    td {
      padding: 10px 0;
      text-align: left;
      vertical-align: middle;
    }

    td:first-child {
      color: #777;
      width: 40%;
    }
  }
}
.s-title{
  color: #333 !important;
  font-size: $text-sm  !important;
  margin-top: 12px !important;
}

.summary-guide {
  text-align: center;
  padding: 60px 20px;
  color: #9aa6a9;
  font-size: 15px;
  font-weight: 500;
  background: #f9fbfb;
  border: 1px dashed #cfe2e2;
  border-radius: 10px;
  transition: opacity 0.3s ease;
}

/* =========================================================
 ✅ 5. 버튼 & 섹션 타이틀
========================================================= */
.submit_btn {
  width: 70%;
  padding: 14px 0;
  font-weight: 600;
  font-size: $button;
  color: #fff;
  background: $color_main;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  transition: 0.3s ease;
  display: block;
  margin: 20px auto 0; /* ✅ 중앙 정렬 */
  width: 70%;

  &:hover {
    background: $color_main_deep;
  }
}

.section-title {
  td {
    font-size: $label-md;
    color: #333;
    font-weight: 600;
    border-bottom: 1px solid #e7e7e7;
    padding: 0;
  }

  & + tr {
    td {
      // padding-top: 10px;
    }
  }
}

/* =========================================================
 ✅ 6. VueDatePicker 커스터마이징
========================================================= */
.mini-btn {
  width: 6rem;
}

.form_group .dp__input_wrap {
  width: 100%;
  border: none !important;
  background: transparent !important;
  box-shadow: none !important;
}

.form_group .dp__main {
  border: none !important;
  background: transparent !important;
  box-shadow: none !important;
}

.form_group .dp__input {
  width: 100%;
  background: transparent !important;
  border: none !important;
  border-bottom: 1px solid #e7e7e7 !important;
  border-radius: 0 !important;
  box-shadow: none !important;
  padding: 10px 10px !important;
  font-size: $label-md !important;
  color: #333 !important;
  transition: border-color 0.25s ease;

  &::placeholder {
    color: #aaa !important;
  }

  &:focus {
    border-bottom: 1px solid $color_main_light !important;
    outline: none !important;
  }
}
:deep(.dp__input) {
  width: 100%;
  background: transparent !important;
  border: none !important;
  border-bottom: 1px solid #e7e7e7 !important;
  border-radius: 0 !important;
  box-shadow: none !important;
  padding: 5px 10px !important;
  font-size: $label-md !important;
  color:rgb(51, 51, 51) !important;
  transition: border-color 0.25s ease;
  
  // ===============왜 폰트색이 연할까===============
  &::placeholder {
    color:#777 !important;
  }

  &:focus {
    border-bottom: 1px solid $color_main_light !important;
    outline: none !important;
  }
}

:deep(.dp__input:focus) {
  border-bottom: 1px solid $color_main_light !important;
  outline: none !important;
}

:deep(.dp__input_icon) {
  display: none !important;
}

.dp__outer_menu_wrap {
  border-radius: 10px !important;
  border: 1px solid #ddd !important;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1) !important;
}
// =============추가ㅓ===========
/* =========================================================
 ✅ VueDatePicker - 달력 팝업 색상만 브랜드 컬러로 변경
========================================================= */
:deep(.dp__outer_menu_wrap),
:deep(.dp__theme_light),
:deep(.dp__theme_dark) {
  --dp-primary-color: #53b4a1 !important; /* ✅ 선택된 날짜 */
  --dp-primary-text-color: #fff !important; /* 선택 텍스트 */
  --dp-hover-color: #449b8a !important; /* hover */
  --dp-hover-text-color: #fff !important; /* hover 텍스트 */
  --dp-range-between-dates-background-color: #eaf8f6 !important; /* 기간 선택 중간색 */
  --dp-border-color: #d2e8e8 !important; /* 달력 팝업 경계선 */
  --dp-menu-border-color: #d2e8e8 !important; /* 메뉴 테두리 */
  --dp-success-color: #53b4a1 !important; /* ✅ Select 버튼 */
  --dp-icon-color: #53b4a1 !important; /* 달력 내부 화살표 아이콘 */
}

/* =========================================================
 ✅ 7. 트랜지션 & 반응형
========================================================= */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.1s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  max-height: 0;
}
.fade-enter-to,
.fade-leave-from {
  opacity: 1;
  max-height: 800px;
}

.Stepper {
  margin-bottom: 40px;
}

/* 반응형 */
@media (max-width: 1024px) {
  .inner {
    .container {
      width: 100%;
      grid-template-columns: 1fr;
      gap: 20px;
      padding: 0;
    }
    .left {
      gap: 20px;
    }
  }

  .form_card {
    padding: 30px 40px;
  }
}

@media (max-width: 450px) {
  .inner {
    .container {
      width: 100%;
      grid-template-columns: 1fr;
      gap: 10px;
    }
    .left {
      gap: 10px;
    }
  }
}

// =====UI통일을위해추가======================
/* =========================================================
 ✅ Reservation2와 동일한 카드 정렬/폭 통일 (추가)
========================================================= */

/* 공통 컨테이너 폭 */
.inner,
.container {
  width: 100%;
  max-width: 1120px; /* Reservation2 동일 기준 */
  margin: 0 auto;
  padding: 0 40px;
  box-sizing: border-box;
}

/* 카드 공통 정렬 및 통일 폭 */
.form_card,
.summary_card {
  width: 100%;
  max-width: none;
  background: #fff;
  border-radius: 10px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.05);
  border: 1px solid #f0f0f0;
  box-sizing: border-box;
}

/* 왼쪽-오른쪽 간격 통일 */
.container {
  display: grid;
  grid-template-columns: 3fr 2fr; /* Reservation2와 동일 */
  gap: 2.5rem;
  align-items: flex-start;
}

/* 제출 버튼도 같은 폭 맞추기 */
.submit_btn {
  width: 70%; /* Reservation2 동일 */
  display: block;
  margin: 20px auto 0;
  background: $color_main;
  color: #fff;
  border: none;
  border-radius: 6px;
  font-weight: 600;
  padding: 14px 0;
  font-size: 1rem;
  cursor: pointer;
  transition: 0.3s ease;

  &:hover {
    background:  #3A8C88;
  }
}

/* 반응형 동일 적용 */
@media (max-width: 1024px) {
  .container {
    grid-template-columns: 1fr;
    gap: 20px;
    max-width: 600px;
  }

  .form_card,
  .summary_card {
    width: 90%;
    margin-inline: auto;
  }

  .submit_btn {
    width: 90%;
    font-size: 1rem;
  }
}


</style>
