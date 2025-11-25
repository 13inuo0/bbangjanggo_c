<template>
  <div>
    <!-- 전체 레이아웃 -->
    <div class="min-h-screen bg-gray-100 dark:bg-gray-900 flex">
      <!-- Font Awesome CDN -->
      <link
        rel="stylesheet"
        href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.1/css/all.min.css"
      />

      <!-- 사이드바 -->
      <div
        class="w-64 min-w-64 h-screen bg-white dark:bg-gray-800 shadow-lg fixed left-0 top-0 flex flex-col"
      >
        <!-- 상단 정보 -->
        <div
          class="p-4 flex-col text-left pl-9 pt-6 border-b border-gray-200 dark:border-gray-700"
        >
          <div class="flex items-end gap-2 mb-6">
            <h1 class="text-3xl font-bold text-gray-800 dark:text-white">
              빵장고
            </h1>
          </div>
          <span class="text-xl font-bold text-gray-800 dark:text-white"
            >김빵장</span
          ><span>님</span>
          <p class="text-xs font-bold py-1">좋은 하루 보내세요!</p>
          <div class="text-xs text-gray-600">{{ currentDate }}</div>
        </div>

        <!-- 네비게이션 메뉴 -->
        <nav class="flex-1 p-4 space-y-2 overflow-y-auto">
          <router-link
            v-for="link in links"
            :key="link.path"
            class="flex items-center px-4 py-4 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors"
            :to="link.path"
            :class="{
              'font-bold text-[#BA8E5F] dark:text-indigo-300': isActive(link.path),
              'text-gray-400 dark:text-gray-500': !isActive(link.path)
            }"
          >
            <i
              :class="link.icon"
              class="mr-3 w-5 text-center"
              :style="{ color: isActive(link.path) ? '#BA8E5F' : '' }"
            ></i>
            {{ link.name }}
          </router-link>

          <!-- 긴급 처리 사항 -->
          <div
            v-if="uncheckedAlerts.length > 0"
            class="mt-4 p-3 bg-red-100 dark:bg-red-800 rounded-lg border border-red-200 dark:border-red-700"
          >
            <h3 class="text-red-700 dark:text-red-300 font-bold text-sm mb-2 flex items-center gap-1">
              <i class="fa-solid fa-triangle-exclamation"></i>
              긴급 처리 사항
            </h3>
            <ul class="text-xs text-gray-700 dark:text-gray-300 space-y-1.5">
              <li
                v-for="(item, idx) in uncheckedAlerts"
                :key="idx"
                class="cursor-pointer hover:underline hover:text-red-700 dark:hover:text-red-400 transition-colors"
                @click="handleAlertClick(item)"
              >
                • {{ item.text }}
              </li>
            </ul>
          </div>
        </nav>

        <!-- 로그아웃 버튼 -->
        <div class="p-4 border-t border-gray-200 dark:border-gray-700">
          <button
            @click="logout"
            class="w-full flex items-center justify-center px-4 py-2 text-gray-400 dark:text-gray-700 rounded-lg hover:bg-gray-100 dark:hover:bg-gray-700 transition-colors"
          >
            <i class="fa-solid fa-right-from-bracket mr-3 w-5"></i>
            로그아웃
          </button>
        </div>
      </div>

      <!-- 오른쪽 메인 영역 -->
      <div class="flex-1 ml-64 min-h-screen">
        <div class="w-full">
          <!-- 사고발생 경고창 배너 -->
          <AlertBanner />

          <!-- 메인 컨텐츠 -->
          <div class="p-3">
            <router-view></router-view>
          </div>
        </div>
      </div>
    </div>

    <!-- 긴급 처리 모달 -->
    <EmergencyModal
      v-model:visible="showEmergencyModal"
      :alerts="alerts"
      @allChecked="handleAllChecked"
    />
  </div>
</template>

<script setup>
import { computed, ref, onMounted } from "vue";
import { useRoute, useRouter } from "vue-router";
import AlertBanner from "@/components/AlertBanner.vue";
import EmergencyModal from "@/components/EmergencyModal.vue";

const route = useRoute();
const router = useRouter();

const links = [
  { name: "대시 보드", path: "/admin/dashboard", icon: "fa-solid fa-house" },
  { name: "예약 관리", path: "/admin/reservation", icon: "fa-solid fa-calendar-check" },
  { name: "기사 관리", path: "/admin/workermanage", icon: "fa-solid fa-truck" },
  { name: "정산관리", path: "/admin/payment", icon: "fa-solid fa-cash-register" },
  { name: "고객 문의 관리", path: "/admin/custormer", icon: "fa-solid fa-comment-dots" },
  { name: "공지 및 알림", path: "/admin/notice", icon: "fa-solid fa-bell" },
  { name: "관리자 설정", path: "/admin/settings", icon: "fa-solid fa-gear" },
];

// 현재 날짜
const currentDate = computed(() => {
  const today = new Date();
  const year = today.getFullYear();
  const month = String(today.getMonth() + 1).padStart(2, "0");
  const day = String(today.getDate()).padStart(2, "0");
  const days = ["일", "월", "화", "수", "목", "금", "토"];
  const dayOfWeek = days[today.getDay()];
  return `${year}.${month}.${day}(${dayOfWeek})`;
});

// 활성 메뉴
const isActive = (path) => route.path === path;

// 로그아웃
const logout = () => router.push("/admin");

// 긴급 처리 사항
const alerts = ref([
  { id: 1, text: "냉장보관소 14번 냉장고 고장으로 작동 불가", checked: false },
  { id: 2, text: "결제 오류", checked: false },
  { id: 3, text: "문의 답변 대기 5건", checked: false },
  { id: 4, text: "중요 승인 대기 3건", checked: false },
]);

const uncheckedAlerts = computed(() =>
  alerts.value.filter((a) => !a.checked)
);

const showEmergencyModal = ref(false);

// 페이지 로드 시 미확인 알림이 있으면 모달 표시
onMounted(() => {
  if (uncheckedAlerts.value.length > 0) {
    showEmergencyModal.value = true;
  }
});

const handleAlertClick = (alert) => {
  showEmergencyModal.value = true;
};

const handleAllChecked = () => {
  alerts.value.forEach(a => a.checked = true);
};
</script>

<style scoped>
/* 아이콘 통일 */
i {
  font-size: 1.125rem;
  width: 1.25rem;
}
</style>