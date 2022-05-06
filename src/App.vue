<script setup lang="ts">
import { Calendar } from "vant";
import dayjs from "dayjs";
import type { CalendarDayItem } from "vant";

const today = dayjs();
const minDate = today.subtract(1, "month").toDate();
const maxDate = today.add(1, "year").toDate();
const dayTimestamp = dayjs("2022-5-4").valueOf();
const nightTimestamp = dayjs("2022-5-5").valueOf();

const formatter = (day: CalendarDayItem) => {
  if (day.date) {
    const timestamp = day.date.valueOf();
    if (((timestamp - dayTimestamp) / 1000 / 24 / 60 / 60) % 4 === 0) {
      day.bottomInfo = "白";
      day.className = "day";
    }
    if (((timestamp - nightTimestamp) / 1000 / 24 / 60 / 60) % 4 === 0) {
      day.bottomInfo = "夜";
      day.className = "night";
    }
    if (timestamp === dayjs(today.format("YYYY-MM-DD")).valueOf()) {
      day.bottomInfo = "今天";
    }
  }
  return day;
};
</script>

<template>
  <Calendar
    title="排班"
    :poppable="false"
    :show-confirm="false"
    :style="{ height: '100vh' }"
    :min-date="minDate"
    :max-date="maxDate"
    readonly
    :formatter="formatter"
    color="#ef4444"
    :first-day-of-week="1"
  />
</template>

<style>
.day {
  color: #10b981;
}
.night {
  color: #8b5cf6;
}
</style>
