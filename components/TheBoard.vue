<script lang="ts" setup>
import { nanoid } from "nanoid";
import draggable from "vuedraggable";
import type { Column, Task } from '@/types';
import { useKeyModifier } from "@vueuse/core";

const ctrlKey = useKeyModifier('Control');

const columns = useLocalStorage<Column[]>('kanban-board', [
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

function createColumn() {
  const column: Column = {
    id: nanoid(),
    title: '',
    tasks: []
  }
  columns.value.push(column);
  nextTick(() => {
    (document.querySelector('.column:last-of-type .title-input') as HTMLInputElement).focus();
  });
}
</script>

<template>
  <div class="flex items-start overflow-x-auto gap-4">
    <draggable
      v-model="columns"
      group="columns"
      item-key="id"
      class="flex gap-4 items-start"
      :animation="150"
      handle=".drag-handle"
    >
      <template #item="{ element: column }: { element: Column }">
        <div
          class="column bg-gray-200 p-5 rounded min-w-[250px]"
        >
          <header class="font-bold mb-4">
            <DragHandle />
            <input
              class="title-input bg-transparent focus:bg-white rounded px-1 w-4/5"
              @keyup.enter="($event.target as HTMLInputElement).blur()"
              @keydown.backspace.delete="column.title === '' ? columns = columns.filter(c => c.id !== column.id) : null"
              type="text"
              v-model="column.title"
            />
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
                <TaskCard :task="task" @delete="column.tasks = column.tasks.filter(t => t.id !== $event)" />
              </div>
            </template>
          </draggable>
          <footer>
            <TaskNew @add="column.tasks.push($event)" />
          </footer>
        </div>
      </template>
    </draggable>
    <button
      @click="createColumn"
      class="bg-gray-200 whitespace-nowrap p-2 rounded opacity-50"
    >
      + Add Another Column
    </button>
  </div>
</template>