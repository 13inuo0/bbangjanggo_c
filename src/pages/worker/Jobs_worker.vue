<template>
  <!-- <div>기사 업무관리 페이지</div> -->
  <!-- 지도를 표시할 div 입니다 -->
  <div id="map" class="w-full min-h-screen static font-[SpokaHanSansNeo] font-normal">
    <div class="w-full h-[360px] bg-gray-100 absolute bottom-0 left-0 z-999 text-center">
      <div class="flex place-content-between mt-[30px] mx-[50px]">
        <div class="flex flex-col gap-4 text-start">
          <div class="flex">
            <p class="text-gray-400 text-[16px] w-[120px] font">예약번호</p>
            <span class="text-gray-800 text-base">20251027-0135</span>
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

        <div class="flex flex-col gap-4">
          <div class="flex">
            <p class="text-gray-400 text-base w-[120px]">픽업지점</p>
            <span class="text-gray-800 text-base">따끈따끈 베이커리</span>
          </div>
          <div class="flex">
            <p class="text-gray-400 text-base w-[120px]">보관지점</p>
            <span class="text-gray-800 text-base">빵장고 [반월당역점]</span>
          </div>
          <div class="flex">
            <p class="text-gray-400 text-base w-[120px]">픽업지점</p>
            <span class="text-gray-800 text-base">상온보관 - 1개</span>
          </div>
        </div>
      </div>
      <button @click="" class="underline text-sm text-gray-700 mt-[30px] cursor-pointer">배송 취소하기</button>
      <button @click="" class="w-[700px] h-[60px] bg-[rgba(186,142,95,1)] text-white rounded-md mt-[30px] cursor-pointer">픽업 완료</button>
    </div>
  </div>
</template>
<script setup>
import { onMounted } from "vue";

onMounted(() => {
  // 카카오 지도 SDK가 로드된 후 실행
  window.kakao.maps.load(() => {
    const mapContainer = document.getElementById("map"); // 지도를 표시할 div

    const mapOption = {
      center: new kakao.maps.LatLng(35.868508, 128.593771), // 지도의 중심좌표
      level: 3, // 지도의 확대 레벨 (숫자가 작을수록 확대)
    };

    //지도를 생성합니다
    const map = new kakao.maps.Map(mapContainer, mapOption);

    // 🚩마커
    // 마커가 표시될 위치입니다
    // 마커를 표시할 위치와 title 객체 배열입니다
    const positions = [
      {
        title: "따끈따끈 베이커리",
        latlng: new kakao.maps.LatLng(35.868508, 128.593771),
      },
      {
        title: "공주당",
        latlng: new kakao.maps.LatLng(35.868006, 128.595659),
      },
      {
        title: "소베",
        latlng: new kakao.maps.LatLng(35.869458, 128.593245),
      },
      {
        title: "네쥬",
        latlng: new kakao.maps.LatLng(35.868691, 128.594742),
      },
      {
        title: "윈드윈",
        latlng: new kakao.maps.LatLng(35.867354, 128.584411),
      },
    ];

    // 마커 이미지 경로 설정
    const imageSrc = "/images/kms/mainpin.png";

    for (var i = 0; i < positions.length; i++) {
      // 마커 이미지 크기
      const imageSize = new kakao.maps.Size(44, 63);

      // 마커 이미지 생성
      const markerImage = new kakao.maps.MarkerImage(imageSrc, imageSize);

      // 마커를 생성합니다
      const marker = new kakao.maps.Marker({
        map: map, // 마커를 표시할 지도
        position: positions[i].latlng, // 마커를 표시할 위치
        title: positions[i].title, // 마커의 타이틀, 마커에 마우스를 올리면 타이틀이 표시됩니다
        image: markerImage, // 마커 이미지 적용
      });
    }
  });
});
</script>
<style scoped></style>
