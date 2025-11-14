<template>
  <div class="space-y-6 bg-gray-50 dark:bg-gray-900 p-6 rounded-lg">
    <!-- 통계 카드 -->
    <DashboardStats :stats="customerStats" />

    <!-- 고객 목록 테이블 -->
    <div class="bg-white dark:bg-gray-800 rounded-lg shadow p-6">
      <h2 class="text-lg font-semibold text-gray-800 dark:text-white mb-4">고객 목록</h2>
      
      <div class="overflow-x-auto">
        <table class="w-full">
          <thead class="bg-gray-50 dark:bg-gray-700">
            <tr>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase">
                고객명
              </th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase">
                연락처
              </th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase">
                가입일
              </th>
              <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 dark:text-gray-400 uppercase">
                상태
              </th>
            </tr>
          </thead>
          <tbody class="divide-y divide-gray-200 dark:divide-gray-700">
            <tr v-for="customer in customers" :key="customer.id" class="hover:bg-gray-50 dark:hover:bg-gray-700">
              <td class="px-6 py-4 text-sm text-gray-900 dark:text-white">
                {{ customer.name }}
              </td>
              <td class="px-6 py-4 text-sm text-gray-600 dark:text-gray-400">
                {{ customer.phone }}
              </td>
              <td class="px-6 py-4 text-sm text-gray-600 dark:text-gray-400">
                {{ customer.joinDate }}
              </td>
              <td class="px-6 py-4">
                <span
                  :class="getStatusClass(customer.status)"
                  class="px-3 py-1 text-xs font-semibold rounded-full"
                >
                  {{ customer.status }}
                </span>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script setup>
import DashboardStats from "@/components/DashboardStats.vue";
import { ref } from "vue";

// 통계 카드 데이터
const customerStats = [
  {
    title: "오늘 예약 건수",
    value: "25건",
    icon: "/images/pjs/c-icon/orders.png"
  },
  {
    title: "진행중 (배차 신청 완료)",
    value: "12 건",
    icon: "/images/pjs/c-icon/dailysales.png"
  },
  {
    title: "입고완료",
    value: "11 건",
    icon: "/images/pjs/c-icon/numreservations.png"
  },
  {
    title: "취소 / 환불건 ",
    value: "1건",
    icon: "/images/pjs/c-icon/visitors.png"
  }
];

// 고객 목록 더미 데이터
const customers = ref([
  { id: 1, name: "김철수", phone: "010-1234-5678", joinDate: "2024-01-15", status: "활성" },
  { id: 2, name: "이영희", phone: "010-2345-6789", joinDate: "2024-02-20", status: "활성" },
  { id: 3, name: "박민수", phone: "010-3456-7890", joinDate: "2024-03-10", status: "비활성" },
  { id: 4, name: "정지은", phone: "010-4567-8901", joinDate: "2024-03-15", status: "활성" }
]);

// 상태 CSS
const getStatusClass = (status) => {
  const statusClasses = {
    활성: "bg-green-100 dark:bg-green-900 text-green-800 dark:text-green-300",
    비활성: "bg-gray-100 dark:bg-gray-900 text-gray-800 dark:text-gray-300"
  };
  return (
    statusClasses[status] || "bg-gray-100 dark:bg-gray-900 text-gray-800 dark:text-gray-300"
  );
};
</script>