<script lang="ts" setup>
import { nanoid } from "nanoid";
import draggable from "vuedraggable";
import type { Column, Task } from '@/types';
import { useKeyModifier } from "@vueuse/core";

const ctrlKey = useKeyModifier('Control');

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
        <draggable
          v-model="column.tasks"
          :group="{ name: 'tasks', pull: ctrlKey ? 'clone' : true }"
          item-key="id"
          :animation="150"
          handle=".drag-handle"
        >
          <template #item="{ element: task }: { element: Task }">
            <div>
              <TaskCard :task="task" />
            </div>
          </template>
        </draggable>
        <footer>
          <TaskNew @add="column.tasks.push($event)" />
        </footer>
      </div>
    </template>
  </draggable>
</template>