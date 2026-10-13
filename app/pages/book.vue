<script setup lang="ts">
import {
  DateFormatter,
  getLocalTimeZone,
  today,
  Time,
  now,
} from "@internationalized/date";

import type { SelectItem } from "@nuxt/ui";
import { ref, watch } from "vue";

const items = ref<SelectItem[]>([
  { label: "8:00 AM", id: 480, disabled: false },
  { label: "8:30 AM", id: 510, disabled: false },
  { label: "9:00 AM", id: 540, disabled: false },
  { label: "9:30 AM", id: 570, disabled: false },
  { label: "10:00 AM", id: 600, disabled: false },
  { label: "10:30 AM", id: 630, disabled: false },
  { label: "11:00 AM", id: 660, disabled: false },
  { label: "11:30 AM", id: 690, disabled: false },
  { label: "12:00 PM", id: 720, disabled: false },
  { label: "12:30 PM", id: 750, disabled: false },
  { label: "1:00 PM", id: 780, disabled: false },
  { label: "1:30 PM", id: 810, disabled: false },
  { label: "2:00 PM", id: 840, disabled: false },
  { label: "2:30 PM", id: 870, disabled: false },
  { label: "3:00 PM", id: 900, disabled: false },
  { label: "3:30 PM", id: 930, disabled: false },
  { label: "4:00 PM", id: 960, disabled: false },
  { label: "4:30 PM", id: 990, disabled: false },
  { label: "5:00 PM", id: 1020, disabled: false },
  { label: "5:30 PM", id: 1050, disabled: false },
  { label: "6:00 PM", id: 1080, disabled: false },
  { label: "6:30 PM", id: 1110, disabled: false },
  { label: "7:00 PM", id: 1140, disabled: false },
  { label: "7:30 PM", id: 1170, disabled: false },
  { label: "8:00 PM", id: 1200, disabled: false },
  { label: "8:30 PM", id: 1230, disabled: false },
  { label: "9:00 PM", id: 1260, disabled: false },
  { label: "9:30 PM", id: 1290, disabled: false },
]);

const open = ref(false);

const df = new DateFormatter("en-GB", {
  dateStyle: "medium",
});

let getDate = shallowRef(today(getLocalTimeZone()));
const minimumDate = shallowRef(today(getLocalTimeZone()));
const currentTime = now(getLocalTimeZone());

function genRoomTimes() {
  let timeOfDay = currentTime.hour * 60 + currentTime.minute;
  if (getDate.value.compare(today(getLocalTimeZone())) == 0) {
    for (let item of items.value) {
      // console.log(item.id);
      if (item.id < timeOfDay) {
        item.disabled = true;
      }
    }
  } else {
    for (let item of items.value) {
      // console.log(item.id);
      if (item.id < timeOfDay) {
        item.disabled = false;
      }
    }
  }
}

genRoomTimes();

watch(getDate, () => {
  console.log("it changed");
  genRoomTimes();
});
</script>

<template>
  <div class="flex justify-center items-center mt-7">
    <UPopover
      v-model:open="open"
      class="mr-6"
      :open-delay="500"
      :close-delay="300"
    >
      <UButton
        color="neutral"
        variant="subtle"
        icon="i-lucide-calendar"
        size="xl"
      >
        {{
          getDate
            ? df.format(getDate.toDate(getLocalTimeZone()))
            : "Select a date"
        }}
      </UButton>

      <template #content>
        <UCalendar
          v-model="getDate"
          class="p-2"
          :min-value="minimumDate"
          @update:model-value="open = false"
        />
      </template>
    </UPopover>
    <UPopover>
      <UButton label="Select Time" color="neutral" variant="subtle" size="xl" />

      <template #content>
        <div class="grid grid-cols-1 gap-4 h-full max-h-130 overflow-y-auto">
          <UButton
            class="px-6 my-0.5 bg-amber-100 rounded-m hover:bg-violet-200 hover:cursor-pointer"
            v-for="item in items"
            :key="item.id"
            :disabled="item.disabled"
            >{{ item.label }}
          </UButton>
        </div>
      </template>
    </UPopover>
  </div>
</template>
