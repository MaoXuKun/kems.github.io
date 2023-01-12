<script setup lang="ts">
import { onMounted } from "vue";
import { Calendar } from "vant";
import dayjs from "dayjs";
import type { CalendarDayItem } from "vant";

const today = dayjs();
const minDate: Date = today.subtract(1, "month").toDate();
const maxDate: Date = today.add(1, "year").toDate();
const dayTimestamp: number = dayjs("2023-1-12").valueOf();
const nightTimestamp: number = dayjs("2023-1-13").valueOf();

const reload: () => void = () => {
  window.location.reload();
};

onMounted(() => {
  const todayStr = today.format("YYYY-MM-DD");
  setInterval(() => {
    if (todayStr !== dayjs().format("YYYY-MM-DD")) {
      reload;
    }
  }, 1000);
});

const formatter = (day: CalendarDayItem) => {
  if (day.date) {
    const timestamp: number = day.date.valueOf();
    if (((timestamp - dayTimestamp) / 1000 / 24 / 60 / 60) % 4 === 0) {
      day.bottomInfo = "白";
      day.className = "day";
    }
    if (((timestamp - nightTimestamp) / 1000 / 24 / 60 / 60) % 4 === 0) {
      day.bottomInfo = "白";
      day.className = "night";
    }
    if (timestamp === dayjs(today.format("YYYY-MM-DD")).valueOf()) {
      day.topInfo = "今天";
    }
  }
  return day;
};
</script>

<template>
  <Calendar
    title="排班"
    :poppable="false"
    confirm-text="刷新"
    :style="{ height: '100vh' }"
    :min-date="minDate"
    :max-date="maxDate"
    readonly
    :formatter="formatter"
    color="#ef4444"
    :first-day-of-week="1"
    :lazy-render="false"
    @confirm="reload"
  />
</template>

<style>
.day {
  color: #10b981;
}
.night {
  color: #8b5cf6;
}
.van-calendar__confirm {
  margin-bottom: 20px;
}
</style>
