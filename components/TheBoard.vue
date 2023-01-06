<script lang="ts" setup>
import { nanoid } from "nanoid";
import draggable from "vuedraggable";
import type { Column } from '@/types';

const columns = ref<Column[]>([
  {
    id: nanoid(),
    title: 'Backlog',
    tasks: [
      {
        id: nanoid(),
        title: 'Task 1',
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: 'Task 2',
        createdAt: new Date(),
      },
      {
        id: nanoid(),
        title: 'Task 3',
        createdAt: new Date(),
      }
    ]
  },
  {
    id: nanoid(),
    title: 'Selected for Dev',
    tasks: []
  },
  {
    id: nanoid(),
    title: 'In Progress',
    tasks: []
  },
  {
    id: nanoid(),
    title: 'QA',
    tasks: []
  },
  {
    id: nanoid(),
    title: 'Done',
    tasks: []
  },
]);
</script>

<template>
  <draggable
    v-model="columns"
    group="columns"
    item-key="id"
    class="flex gap-4 overflow-x-auto items-start"
    :animation="150"
    handle=".drag-handle"
  >
    <template #item="{ element: column }: { element: Column }">
      <div
        class="column bg-gray-200 p-5 rounded min-w-[250px]"
      >
        <header class="font-bold mb-4">
          <DragHandle />
          {{ column.title }}
        </header>
        <TaskCard v-for="task in column.tasks" :key="task.id" :task="task" />
        <footer>
          <button
            type="button"
            class="text-gray-500"
          >+ Add a Card</button>
        </footer>
      </div>
    </template>
  </draggable>
</template>