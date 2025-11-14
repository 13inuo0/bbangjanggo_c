<template>
  <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
    <!-- 통계 카드 컴포넌트-->
    <div
      v-for="(card, index) in stats"
      :key="index"
      class="bg-white dark:bg-gray-800 rounded-lg shadow-sm p-5 hover:shadow-md transition-shadow border border-gray-200 dark:border-gray-700"
    >
      <div class="flex items-center justify-between">
        <!-- 왼쪽: 텍스트 -->
        <div class="flex flex-col">
          <!-- 타이틀 -->
          <p class="text-xs text-gray-500 dark:text-gray-400 mb-2">
            {{ card.title }}
          </p>
          <!-- 값 -->
          <p class="text-2xl font-bold text-gray-900 dark:text-white">
            {{ card.value }}
          </p>
        </div>

        <!-- 오른쪽: 아이콘 -->
        <div class="flex-shrink-0">
          <img 
            :src="card.icon" 
            :alt="card.title"
            class="w-12 h-12 object-contain"
          />
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