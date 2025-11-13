<template>
  <div>
    <!-- 고정바 -->
    <div class="fixed w-full h-17 bg-white items-center">
      <h1 class="text-3xl font-bold text-gray-800 dark:text-white py-4 p-10">빵장고</h1>
    </div>
    <!-- 왼쪽 사이드 바 -->
    <div class="min-h-screen bg-gray-100 dark:bg-gray-900">
      <!-- Font Awesome CDN 추가 -->
      <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css" />
      <!-- 사이드 바 -->
      <div class="fixed left-0 top-20 h-150 w-57 bg-white dark:bg-gray-800 shadow-lg rounded-r-xl">
        <div class="flex flex-col h-150">
          <!-- 로고 -->
          <div class="p-4 border-b border-gray-200 dark:border-gray-700 flex-col text-left pl-9">
            <p class="text-xs font-bold pl-1 pb-0.5">좋은 하루 보내세요!</p>
            <span class="text-xl font-bold text-gray-800 dark:text-white text-center">공주당 매니저</span>
            <span>님</span>
          </div>
          <!-- 네비게이션 메뉴 -->
          <nav class="flex-1 p-4 space-y-2">
            <router-link
              v-for="link in links"
              :key="link.path"
              class="flex items-center px-4 py-2 text-gray-700 dark:text-gray-300 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors"
              :to="link.path"
              :class="{
                'bg-indigo-100 dark:bg-indigo-900 text-indigo-700 dark:text-indigo-300': isActive(link.path),
              }"
            >
              <img :src="link.icon" class="mr-3 w-5 h-5" />{{ link.name }}
            </router-link>
          </nav>

          <!-- 로그아웃 버튼 -->
          <div class="p-4 border-t border-gray-200 dark:border-gray-700">
            <button
              @click="logout"
              class="w-full flex items-center justify-center px-4 py-2 text-gray-700 dark:text-gray-300 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors"
            >
              <img src="/images/pjs/c-icon/logoout.png" class="mr-3 w-5 h-5" />
              로그아웃
            </button>
          </div>
        </div>
      </div>

      <!-- 네비게이션 메뉴 -->
      <!-- 오른쪽 내용 -->
      <div class="ml-64 min-h-creen">
        <div class="p-8">
          <router-view></router-view>
        </div>
      </div>
    </div>
  </div>
</template>
<script setup>
import { useRoute, useRouter } from "vue-router";

const route = useRoute();
const router = useRouter();
// fas fa-chart-line
const links = [
  { name: "대시보드", path: "/admin/dashboard", icon: "/images/pjs/c-icon/dashboard.png" },
  {
    name: "예약관리",
    path: "/admin/reservation",
    icon: "/images/pjs/c-icon/reservation.png",
  },
  { name: "기사관리", path: "/admin/workermanage", icon: "/images/pjs/c-icon/delivery.png" },
  //   { name: "고객관리", path: "/admin/customers", icon: "fas fa-users" },
  //   { name: "문의관리", path: "/admin/inquiries", icon: "fas fa-inbox" },
  //   { name: "고객 소통", path: "/admin/customer-communication", icon: "fas fa-comments" },
  { name: "설정", path: "/admin/settings", icon: "/images/pjs/c-icon/setting.png" },
];

// 현재 경로에 따른 활성화 상태
const isActive = (path) => route.path === path;
const logout = () => {
  // 로그아웃 처리
  router.push("/admin");
};
</script>
<style scoped>
@media (max-width: 768px) {
  .fixed {
    position: relative;
    width: 100%;
    height: auto;
  }
  .ml-64 {
    margin-left: 0;
  }
}
</style>
