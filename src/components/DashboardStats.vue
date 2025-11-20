<!-- DashboardStats.vue -->
<template>
  <div class="grid grid-cols-2 md:grid-cols-4 gap-3 mb-3 min-w-246">
    <!-- Font Awesome CDN 추가 -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" />
    
    <!-- 통계 카드 컴포넌트-->
    <div
      v-for="(card, index) in stats"
      :key="index"
      class="bg-white dark:bg-gray-800 rounded-lg shadow-xs px-9 py-4"
    >
      <div class="flex items-center justify-between">
        <!-- 왼쪽: 텍스트 -->
        <div class="flex flex-col justify-center">
          <!-- 타이틀 -->
          <p class="text-xs font-semibold text-gray-600 dark:text-gray-400 mb-1 whitespace-nowrap">
            {{ card.title }}
          </p>
          <!-- 값 -->
          <p class="text-xl font-bold text-gray-900 dark:text-white">
            {{ card.value }}
          </p>
        </div>

        <!-- 오른쪽: 아이콘 (동그라미 배경) -->
        <div 
          :class="[
            'w-12 h-12 rounded-full flex items-center justify-center flex-shrink-0',
            card.bgColor
          ]"
        >
          <i 
            :class="card.icon"
            class="text-xl text-gray-400 dark:text-gray-500"
          ></i>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  stats: {
    type: Array,
    required: true,
    default: () => []
  }
});
</script>

<style scoped>
/* 아이콘 크기 조정 */
i {
  font-size: 1.5rem; /* 24px */
}
</style>