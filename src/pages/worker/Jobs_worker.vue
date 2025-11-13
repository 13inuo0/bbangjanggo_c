<template>
기사 업무관리 페이지
  <!-- 지도를 표시할 div 입니다 -->
<div id="map" class="w-full h-350px"></div>

</template>
<script setup>
import { onMounted } from "vue";

onMounted(() => {
  // 카카오 지도 SDK가 로드된 후 실행
  window.kakao.maps.load(() => {
    const container = document.getElementById("map"); // 지도를 표시할 div
    const options = {
      center: new kakao.maps.LatLng(33.450701, 126.570667), // 지도의 중심좌표
      level: 3, // 지도의 확대 레벨 (숫자가 작을수록 확대)
    };
    // 지도를 표시할 div와  지도 옵션으로  지도를 생성합니다
    const map = new kakao.maps.Map(container, options);
  });
});
</script>
<style scoped></style>