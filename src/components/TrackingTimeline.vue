<template>
  <div class="border-t border-gray-200 bg-gray-700 p-6" v-if="result?.events?.length">
    <h3 class="font-bold text-gray-800 mb-4 flex items-center">
      <i class="fas fa-history text-primary ml-2"></i>
      آخرین وضعیت
    </h3>

    <div class="space-y-4">
      <!-- آیتم‌های وضعیت -->
      <div
        v-for="(event, index) in result.events"
        :key="index"
        class="flex items-start"
      >
        <div class="relative">
          <!-- نقطه وضعیت -->
          <div
            class="h-4 w-4 rounded-full mt-1"
            :class="{
              'bg-primary': index === 0,
              'bg-green-300': index !== 0
            }"
          ></div>
          <!-- خط اتصال (به جز برای آخرین آیتم) -->
          <div
            v-if="index !== result.events.length - 1"
            class="absolute left-1/2 top-5 bottom-0 w-0.5 transform -translate-x-1/2"
            :class="{
              'bg-primary': index === 0,
              'bg-green-300': index !== 0
            }"
          ></div>
        </div>

        <div class="mr-3 flex-1">
          <div class="bg-white p-3 rounded-lg shadow-sm border border-gray-100">
            <div class="flex justify-between items-start">
              <h4 class="font-medium text-gray-800">{{ event.description }}</h4>
              <span class="text-xs text-gray-500">
                {{ event.friendly_event_date || event.local_event_date }} - {{ event.event_time }}
              </span>
            </div>
            <p
              v-if="event.location"
              class="text-sm text-gray-600 mt-1"
            >
              <i class="fas fa-map-marker-alt text-xs text-gray-400 ml-1"></i>
              {{ event.location }}
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- حالت عدم وجود داده -->
  <div v-else-if="result" class="p-6 text-center text-gray-500">
    اطلاعات وضعیتی یافت نشد
  </div>
</template>

<script setup>
// تعریف props برای دریافت result از کامپوننت والد
const props = defineProps({
  result: {
    type: Object,
    default: () => ({ events: [] })
  }
});
</script>
