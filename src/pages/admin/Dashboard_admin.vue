<template>
  <div class="space-y-6 bg-gray-50 dark:bg-gray-900 p-6 rounded-lg">
    <!-- 통계 카드 -->
    <DashboardStats :stats="stats" />

    <!-- 차트와 최근 예약 -->
    <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
      <!-- 차트 -->
      <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
        <h2 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">예약 추이</h2>
        <div class="h-64">
          <Chart />
        </div>
      </div>

      <!-- 최근 예약 -->
      <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
        <h2 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">최근 예약</h2>
        <div class="space-y-4">
          <div
            v-for="reservation in recentReservations"
            :key="reservation.id"
            class="flex items-center justify-between p-4 bg-gray-50 dark:bg-gray-700 rounded-lg"
          >
            <div>
              <p class="font-medium text-gray-900 dark:text-white">
                {{ reservation.customerName }}
              </p>
              <p class="text-sm text-gray-500 dark:text-gray-400">{{ reservation.date }}</p>
            </div>
            <span
              :class="getStatusClass(reservation.status)"
              class="px-3 py-1 text-xs font-semibold rounded-full"
            >
              {{ reservation.status }}
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import Chart from "@/components/Chart.vue";
import DashboardStats from "@/components/DashboardStats.vue";
import { ref } from "vue";

// 통계 카드 데이터 (이미지 URL 사용)
const stats = [
  {
    title: "실시간 주문 수",
    value: "25건",
    icon: "/images/pjs/c-icon/orders.png" 
  },
  {
    title: "일 매출",
    value: "958,000 원",
    icon: "/images/pjs/c-icon/dailysales.png" 
  },
  {
    title: "일 예약수 / 월간 총  예약 수",
    value: "31 건 / 128 건",
    icon: "/images/pjs/c-icon/reservation.png" 
  },
  {
    title: "일 방문자수",
    value: "85 병",
    icon: "/images/pjs/c-icon/visitors.png" 
  }
];

// 최근 예약
const recentReservations = ref([
  { id: 1, customerName: "김철수", date: "2024-03-20", status: "확정" },
  { id: 2, customerName: "이영희", date: "2024-03-21", status: "대기" },
  { id: 3, customerName: "박민수", date: "2024-03-22", status: "취소" },
  { id: 4, customerName: "정지은", date: "2024-03-23", status: "확정" }
]);

// 예약 상태 CSS
const getStatusClass = (status) => {
  const statusClasses = {
    확정: "bg-green-100 dark:bg-green-900 text-green-800 dark:text-green-300",
    대기: "bg-yellow-100 dark:bg-yellow-900 text-yellow-800 dark:text-yellow-300",
    취소: "bg-red-100 dark:bg-red-900 text-red-800 dark:text-red-300"
  };
  return (
    statusClasses[status] || "bg-gray-100 dark:bg-gray-900 text-gray-800 dark:text-gray-300"
  );
};
</script>