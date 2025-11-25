<template>
  <div v-if="visible" class="fixed inset-0 flex items-center justify-center z-50">
    <div class="fixed inset-0 bg-black/50"></div>

    <div class="bg-white dark:bg-gray-800 rounded-lg shadow-2xl p-6 w-96 relative z-10 animate-fadeIn">
      <h2 class="text-xl font-bold text-red-600 mb-4 flex items-center gap-2">
        <i class="fa-solid fa-triangle-exclamation"></i>
        긴급 처리 알림
      </h2>

      <div class="space-y-3 max-h-64 overflow-y-auto">
        <div
          v-for="(item, index) in localAlerts"
          :key="index"
          @click="markAsChecked(index)"
          class="border rounded p-3 flex justify-between items-center cursor-pointer transition-all"
          :class="item.checked ? 'bg-green-50 border-green-300 dark:bg-green-900/30' : 'bg-red-50 border-red-200 hover:bg-red-100 dark:bg-red-900/30'"
        >
          <div class="text-sm text-gray-800 dark:text-gray-200">{{ item.text }}</div>
          <i v-if="item.checked" class="fa-solid fa-circle-check text-green-600 text-lg ml-2"></i>
        </div>
      </div>

      <div class="mt-6 flex justify-end">
        <button
          @click="close"
          :disabled="!allChecked"
          class="px-4 py-2 rounded font-medium transition-all"
          :class="allChecked ? 'bg-red-600 text-white hover:bg-red-700' : 'bg-gray-300 text-gray-500 cursor-not-allowed'"
        >
          닫기
        </button>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed, watch } from "vue";

const props = defineProps({
  visible: Boolean,
  alerts: Array
});

const emit = defineEmits(["update:visible", "allChecked"]);

const localAlerts = ref([]);

watch(() => props.alerts, (newAlerts) => {
  localAlerts.value = newAlerts.map(a => ({ ...a }));
}, { immediate: true, deep: true });

const markAsChecked = (index) => {
  localAlerts.value[index].checked = true;
};

const allChecked = computed(() => localAlerts.value.every(a => a.checked));

const close = () => {
  if (allChecked.value) {
    emit("allChecked");
    emit("update:visible", false);
  }
};
</script>

<style scoped>
@keyframes fadeIn {
  0% { opacity: 0; transform: translateY(-10px); }
  100% { opacity: 1; transform: translateY(0); }
}
.animate-fadeIn {
  animation: fadeIn 0.3s ease-out;
}
</style>