<template>
  <mp-slide>
    <template #header>
      <h1>План разработки MVP: Teamly Kanban</h1>
    </template>

    <mp-gantt
      :items="tasks"
      start-key="start"
      end-key="end"
      label-key="label"
      color-key="color"
      variant="filled"
      :grid="true"
      dense
    />
  </mp-slide>
</template>

<script setup lang="ts">
import { ref } from "vue";

const tasks = ref([
  {
    label: "Анализ требований",
    start: "2024-01-10",
    end: "2024-01-12",
    color: "secondary",
  },
  {
    label: "Создание ТЗ",
    start: "2024-01-12",
    end: "2024-01-15",
    color: "secondary",
  },
  {
    label: "Дизайн схемы БД",
    start: "2024-01-15",
    end: "2024-01-18",
    color: "tertiary",
  },
  {
    label: "Миграции БД",
    start: "2024-01-18",
    end: "2024-01-20",
    color: "tertiary",
  },
  {
    label: "Модели пользователей",
    start: "2024-01-20",
    end: "2024-01-22",
    color: "primary",
  },
  {
    label: "Модели задач",
    start: "2024-01-22",
    end: "2024-01-24",
    color: "primary",
  },
  {
    label: "Модели колонок",
    start: "2024-01-24",
    end: "2024-01-26",
    color: "primary",
  },
  {
    label: "API аутентификации",
    start: "2024-01-26",
    end: "2024-01-30",
    color: "primary",
  },
  {
    label: "API задач (CRUD)",
    start: "2024-01-30",
    end: "2024-02-03",
    color: "primary",
  },
  {
    label: "API колонок",
    start: "2024-02-03",
    end: "2024-02-06",
    color: "primary",
  },
  {
    label: "UI компоненты (Button, Input)",
    start: "2024-01-18",
    end: "2024-01-25",
    color: "tertiary",
  },
  {
    label: "Карточка задачи",
    start: "2024-01-25",
    end: "2024-01-28",
    color: "tertiary",
  },
  {
    label: "Колонка Kanban",
    start: "2024-01-28",
    end: "2024-02-01",
    color: "tertiary",
  },
  {
    label: "Drag & Drop основа",
    start: "2024-02-01",
    end: "2024-02-05",
    color: "secondary",
  },
  {
    label: "Интеграция API (список задач)",
    start: "2024-02-10",
    end: "2024-02-13",
    color: "primary",
  },
  {
    label: "Интеграция API (обновление)",
    start: "2024-02-13",
    end: "2024-02-16",
    color: "primary",
  },
  {
    label: "Обработка ошибок",
    start: "2024-02-16",
    end: "2024-02-18",
    color: "tertiary",
  },
  {
    label: "Unit тесты компонентов",
    start: "2024-02-18",
    end: "2024-02-21",
    color: "tertiary",
  },
  {
    label: "E2E тесты основного потока",
    start: "2024-02-21",
    end: "2024-02-25",
    color: "tertiary",
  },
  {
    label: "Багфикс и оптимизация",
    start: "2024-02-25",
    end: "2024-02-28",
    color: "tertiary",
  },
]);
</script>

<style scoped></style>
