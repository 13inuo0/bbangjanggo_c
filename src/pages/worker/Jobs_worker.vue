<template>
  <div class="w-full max-w-[768px] mx-auto font-[SpokaHanSansNeo] font-normal relative" style="height: 100vh">
    <!-- 배송 목록 화면 -->
    <div v-show="showDeliveryList" class="w-full h-full bg-white overflow-y-auto pb-25">
      <div>
        <div class="h-[100px] w-full max-w-[750px] fixed bg-white z-999 pl-[25px]">
          <div class="w-full max-w-[600px] flex flex-col gap-4">
            <!-- 배송 완료건 확인 정보 -->
            <div class="flex place-content-between items-center mt-7">
              <p class="text-xl">{{ remainingCount }}건만 더 하면 배달 완료! 힘내세요!</p>
              <span>{{ remainingCount }}/{{ totalCount }}</span>
            </div>
            <!-- 프로그레스바 -->
            <div class="w-full mb-9">
              <div class="w-full h-3 bg-gray-200 rounded-full">
                <div
                  class="h-full bg-[#50311D] rounded-full transition-all duration-500 ease-out"
                  :style="{ width: progressPercent + '%' }"
                ></div>
              </div>
            </div>
          </div>
        </div>

        <!-- 배송 카드들 -->
        <div class="space-y-4 pt-[120px] px-6">
          <div
            v-for="delivery in sortedDeliveryList"
            :key="delivery.reservationNo"
            class="border border-gray-300 rounded-lg transition-opacity duration-300"
            :class="{ 'opacity-60': delivery.status === 'completed' }"
          >
            <!-- 배송카드 상단 예약정보 및 손님정보 -->
            <div
              class="flex justify-between items-start p-5 rounded-t-lg"
              :class="delivery.status === 'completed' ? 'bg-gray-300' : 'bg-[#ba8e5f]'"
            >
              <p class="text-sm text-gray-50">예약 번호: {{ delivery.reservationNo }}</p>
              <p class="text-sm text-gray-50 space-y-1">{{ delivery.customerName }} · {{ delivery.phone }}</p>
            </div>
            <div class="flex justify-between items-end p-5">
              <!-- 픽업장소 및 배달장소 -->
              <div class="flex flex-col items-start gap-[15px]">
                <div class="flex">
                  <div class="relative bg-[#ba8e5f] w-[40px] h-[40px] rounded-4xl mr-4">
                    <p
                      class="absolute top-[50%] left-[50%] -translate-x-[50%] -translate-y-[50%] whitespace-nowrap text-[13px] text-white font-[SpokaHanSansNeo]"
                    >
                      픽업
                    </p>
                  </div>
                  <p class="text-base my-2">{{ delivery.storeName }}</p>
                </div>
                <div class="flex">
                  <div class="relative bg-[#50311d] w-[40px] h-[40px] rounded-4xl mr-4">
                    <p
                      class="absolute top-[50%] left-[50%] -translate-x-[50%] -translate-y-[50%] whitespace-nowrap text-[13px] text-white font-[SpokaHanSansNeo]"
                    >
                      배달
                    </p>
                  </div>
                  <p class="text-base my-2">보관지점: {{ delivery.storage }}</p>
                </div>
              </div>
              <!-- 배달리스트 상태 버튼 -->
              <div class="text-sm text-gray-600 space-y-1">
                <button
                  @click="handleStatusChange(delivery)"
                  :disabled="delivery.status === 'completed'"
                  class="px-4 py-2 text-white text-sm rounded transition-all duration-300"
                  :class="[
                    getStatusClass(delivery.status),
                    delivery.status === 'completed'
                      ? 'cursor-not-allowed'
                      : 'cursor-pointer hover:opacity-80 active:scale-95',
                  ]"
                >
                  {{ getStatusText(delivery.status) }}
                </button>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 지도 화면 (기존) -->
    <div v-show="!showDeliveryList" class="absolute inset-0 w-full h-full" style="overflow: hidden">
      <div id="map" class="w-full h-full"></div>

      <transition name="slide-up">
        <div v-if="showPanel" class="w-full h-[360px] bg-gray-100 absolute bottom-0 left-0 z-[999] text-center">
          <div class="flex place-content-between mt-[30px] mx-[50px]">
            <div class="flex flex-col gap-4 text-start">
              <div class="flex">
                <p class="text-gray-400 text-[16px] w-[120px] font">예약번호</p>
                <span class="text-gray-800 text-base">{{ selectedMarker?.reservationNo || "20251027-0135" }}</span>
              </div>
              <div class="flex">
                <p class="text-gray-400 text-base w-[120px]">이름</p>
                <span class="text-gray-800 text-base">김빵장</span>
              </div>
              <div class="flex">
                <p class="text-gray-400 text-base w-[120px]">전화번호</p>
                <span class="text-gray-800 text-base">010-1234-5678</span>
              </div>
            </div>

            <div class="flex flex-col gap-4 text-start">
              <div class="flex">
                <p class="text-gray-400 text-base w-[120px]">픽업지점</p>
                <span class="text-gray-800 text-base">{{ selectedMarker?.title || "따끈따끈 베이커리" }}</span>
              </div>
              <div class="flex">
                <p class="text-gray-400 text-base w-[120px]">보관지점</p>
                <span class="text-gray-800 text-base">빵장고 [반월당역점]</span>
              </div>
              <div class="flex">
                <p class="text-gray-400 text-base w-[120px]">상태</p>
                <span
                  class="text-base font-semibold"
                  :class="{
                    'text-[#E67E50]': deliveryStatus === 'pickup',
                    'text-[#00ADD8]': deliveryStatus === 'delivering',
                    'text-gray-400': deliveryStatus === 'completed',
                  }"
                >
                  {{ statusText }}
                </span>
              </div>
            </div>
          </div>

          <button
            v-if="deliveryStatus !== 'completed'"
            @click="handleCancel"
            class="underline text-sm text-gray-700 mt-[30px] cursor-pointer hover:text-gray-900"
          >
            배송 취소하기
          </button>

          <div class="mt-[30px]">
            <button
              v-if="deliveryStatus === 'pickup'"
              @click="handlePickupComplete"
              class="w-[700px] h-[60px] bg-[#E67E50] text-white rounded-md cursor-pointer hover:bg-[#D66940] transition-colors"
            >
              픽업 완료
            </button>

            <button
              v-else-if="deliveryStatus === 'delivering'"
              @click="handleDeliveryComplete"
              class="w-[700px] h-[60px] bg-[#00ADD8] text-white rounded-md cursor-pointer hover:bg-[#15A4C8] transition-colors"
            >
              배송 완료
            </button>

            <button
              v-else-if="deliveryStatus === 'completed'"
              @click="handleClose"
              class="w-[700px] h-[60px] bg-gray-400 text-white rounded-md cursor-not-allowed"
            >
              ✓ 배송 완료됨 - 닫기
            </button>
          </div>
        </div>
      </transition>
    </div>

    <!-- 토글 버튼 -->
    <div
      @click="workToggle"
      class="w-[50px] h-[50px] bg-[#50311D] absolute top-[35px] right-[40px] rounded-full z-[999] transform translate-z-0 cursor-pointer"
    >
      <i
        :class="showDeliveryList ? 'fa-map' : 'fa-bars'"
        class="fa-solid absolute top-[50%] left-[50%] w-full -translate-x-[50%] -translate-y-[50%] text-white text-center text-2xl"
      ></i>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, onMounted } from "vue";

const showPanel = ref(false);
const selectedMarker = ref(null);
const selectedMarkerInstance = ref(null);
const deliveryStatus = ref("pickup");
const showDeliveryList = ref(false);
let map = null;
let markers = [];

// 배송 목록 더미 데이터
const deliveryList = ref([
  {
    reservationNo: "20251027-0135",
    storeName: "따끈따끈 베이커리",
    customerName: "김빵장",
    phone: "010-1234-5678",
    storage: "빵장고 [반월당역점]",
    status: "pickup",
  },
  {
    reservationNo: "20251027-0136",
    storeName: "공주당",
    customerName: "이빵순",
    phone: "010-2345-6789",
    storage: "빵장고 [중앙로점]",
    status: "delivering",
  },
  {
    reservationNo: "20251027-0137",
    storeName: "소베",
    customerName: "박빵돌",
    phone: "010-3456-7890",
    storage: "빵장고 [동성로점]",
    status: "completed",
  },
  {
    reservationNo: "20251027-0138",
    storeName: "네쥬",
    customerName: "최빵희",
    phone: "010-4567-8901",
    storage: "빵장고 [반월당역점]",
    status: "pickup",
  },
  {
    reservationNo: "20251027-0139",
    storeName: "윈드윈",
    customerName: "정빵식",
    phone: "010-5678-9012",
    storage: "빵장고 [서문시장점]",
    status: "pickup",
  },
]);

const statusText = computed(() => {
  const statusMap = {
    pickup: "픽업 대기중",
    delivering: "배송 중",
    completed: "배송 완료",
  };
  return statusMap[deliveryStatus.value];
});

// 배송 목록에서 사용할 상태 텍스트 가져오기
const getStatusText = (status) => {
  const statusMap = {
    pickup: "픽업 대기중",
    delivering: "배송 중",
    completed: "배송 완료",
  };
  return statusMap[status];
};

// 배송 목록에서 사용할 상태 색상 클래스 가져오기
const getStatusClass = (status) => {
  const statusClass = {
    pickup: "bg-[#E67E50]",
    delivering: "bg-[#00ADD8]",
    completed: "bg-gray-400",
  };
  return statusClass[status];
};

// 완료된 배송 건수
const completedCount = computed(() => {
  return deliveryList.value.filter((d) => d.status === "completed").length;
});

// 전체 배송 건수
const totalCount = computed(() => {
  return deliveryList.value.length;
});

// 프로그레스 바 퍼센트 계산
const progressPercent = computed(() => {
  return (completedCount.value / totalCount.value) * 100;
});

// 배송 완료되지 않은 건수 계산
const remainingCount = computed(() => {
  return deliveryList.value.filter((d) => d.status !== "completed").length;
});

// 배송 목록 정렬 (진행 중인 건이 위로, 완료된 건이 아래로)
const sortedDeliveryList = computed(() => {
  return [...deliveryList.value].sort((a, b) => {
    // 상태 우선순위: pickup(0) < delivering(1) < completed(2)
    const statusOrder = {
      pickup: 0,
      delivering: 1,
      completed: 2,
    };
    return statusOrder[a.status] - statusOrder[b.status];
  });
});

const handleStatusChange = (delivery) => {
  if (delivery.status === "pickup") {
    delivery.status = "delivering";
  } else if (delivery.status === "delivering") {
    delivery.status = "completed";
  }

  // 지도 마커 업데이트
  updateMarkerOpacity(delivery);
};

// 마커 투명도 업데이트 함수
const updateMarkerOpacity = (delivery) => {
  const marker = markers.find((m) => m.getTitle() === delivery.storeName);
  if (marker) {
    const markerImage = marker.getImage();
    if (delivery.status === "completed") {
      // 완료된 마커는 투명도 적용
      marker.setOpacity(0.4);
    } else {
      // 진행 중인 마커는 불투명하게
      marker.setOpacity(1);
    }
  }
};

const handleCancel = () => {
  if (confirm("정말 배송을 취소하시겠습니까?")) {
    if (selectedMarkerInstance.value) {
      selectedMarkerInstance.value.setMap(null);
    }
    showPanel.value = false;
    deliveryStatus.value = "pickup";
    selectedMarker.value = null;
    selectedMarkerInstance.value = null;
  }
};

const handlePickupComplete = () => {
  deliveryStatus.value = "delivering";

  // deliveryList에서 해당 배송 정보 찾아서 업데이트
  const delivery = deliveryList.value.find((d) => d.reservationNo === selectedMarker.value?.reservationNo);
  if (delivery) {
    delivery.status = "delivering";
    updateMarkerOpacity(delivery);
  }
};

const handleDeliveryComplete = () => {
  deliveryStatus.value = "completed";

  // deliveryList에서 해당 배송 정보 찾아서 업데이트
  const delivery = deliveryList.value.find((d) => d.reservationNo === selectedMarker.value?.reservationNo);
  if (delivery) {
    delivery.status = "completed";
    updateMarkerOpacity(delivery);
  }

  setTimeout(() => {
    handleClose();
  }, 2000);
};

const handleClose = () => {
  // 마커는 삭제하지 않고 그대로 둠 (이미 updateMarkerOpacity로 회색 처리됨)
  showPanel.value = false;
  deliveryStatus.value = "pickup";
  selectedMarker.value = null;
  selectedMarkerInstance.value = null;
};

const workToggle = () => {
  showDeliveryList.value = !showDeliveryList.value;
  if (showDeliveryList.value) {
    showPanel.value = false;
  }
};

const initMap = () => {
  window.kakao.maps.load(() => {
    const mapContainer = document.getElementById("map");
    if (!mapContainer) return;

    const mapOption = {
      center: new kakao.maps.LatLng(35.868508, 128.593771),
      level: 3,
    };

    map = new kakao.maps.Map(mapContainer, mapOption);

    const positions = [
      {
        title: "따끈따끈 베이커리",
        latlng: new kakao.maps.LatLng(35.868508, 128.593771),
        reservationNo: "20251027-0135",
        imageSrc: "/images/kms/mainpin.png",
        imageSize: { width: 44, height: 63 },
      },
      {
        title: "공주당",
        latlng: new kakao.maps.LatLng(35.868006, 128.595659),
        reservationNo: "20251027-0136",
        imageSrc: "/images/pje/deliver_pin1.png",
        imageSize: { width: 44, height: 63 },
      },
      {
        title: "소베",
        latlng: new kakao.maps.LatLng(35.869458, 128.593245),
        reservationNo: "20251027-0137",
        imageSrc: "/images/pje/deliver_pin2.png",
        imageSize: { width: 44, height: 63 },
      },
      {
        title: "네쥬",
        latlng: new kakao.maps.LatLng(35.868691, 128.594742),
        reservationNo: "20251027-0138",
        imageSrc: "/images/pje/deliver_pin3.png",
        imageSize: { width: 44, height: 63 },
      },
      {
        title: "윈드윈",
        latlng: new kakao.maps.LatLng(35.867354, 128.584411),
        reservationNo: "20251027-0139",
        imageSrc: "/images/pje/deliver_pin4.png",
        imageSize: { width: 44, height: 63 },
      },
    ];

    markers = [];
    positions.forEach((info) => {
      const markerImageSrc = info.imageSrc;
      const markerImageSize = new kakao.maps.Size(info.imageSize.width, info.imageSize.height);
      const markerImage = new kakao.maps.MarkerImage(markerImageSrc, markerImageSize);

      const marker = new kakao.maps.Marker({
        map: map,
        position: info.latlng,
        title: info.title,
        image: markerImage,
      });

      // deliveryList에서 해당 마커의 초기 상태 찾기
      const deliveryInfo = deliveryList.value.find((d) => d.storeName === info.title);
      if (deliveryInfo && deliveryInfo.status === "completed") {
        marker.setOpacity(0.4);
      }

      kakao.maps.event.addListener(marker, "click", function () {
        selectedMarker.value = info;
        selectedMarkerInstance.value = marker;
        showPanel.value = true;

        // 클릭한 마커의 현재 상태를 deliveryList에서 가져오기
        const currentDelivery = deliveryList.value.find((d) => d.reservationNo === info.reservationNo);
        deliveryStatus.value = currentDelivery ? currentDelivery.status : "pickup";
      });

      markers.push(marker);
    });
  });
};

onMounted(() => {
  initMap();
});
</script>

<style scoped>
.slide-up-enter-active {
  transition: transform 0.3s ease-out;
}

.slide-up-leave-active {
  transition: transform 0.3s ease-in;
}

.slide-up-enter-from {
  transform: translateY(100%);
}

.slide-up-leave-to {
  transform: translateY(100%);
}
</style>
