<script setup lang="ts">
import type { Task } from "@/types";
import { nanoid } from "nanoid";

const emit = defineEmits<{
  (e: "add", payload: Task): void;
}>();

const focused = ref(false);
const title = ref('');

function createTask(e: Event) {
  if (title.value.trim()) {
    e.preventDefault();
    emit("add", {
      id: nanoid(),
      title: title.value.trim(),
      createdAt: new Date(),
    } as Task)
  }

  title.value = "";0
}
</script>

<template>
  <div>
    <textarea
      v-model="title"
      @keydown.tab="createTask"
      @keyup.enter="createTask"
      rows="1"
      class="focus:bg-white focus:shadow resize-none rounded w-full border-none bg-transparent p-2 cursor-pointer overflow-hidden"
      :class="{
        'h-7': !focused,
        'h-20': focused
      }"
      style="outline: none !important"
      @focus="focused = true"
      @blur="focused = false"
      :placeholder="!focused ? '+ Add a Card' : 'Enter a title for this card'"
    ></textarea>
  </div>
</template>