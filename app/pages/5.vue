<template>
  <mp-slide>
    <template #header>
      <h1>План разработки</h1>
    </template>

    <mp-gantt
      :items="tasks"
      start-key="start"
      end-key="end"
      label-key="label"
      color-key="color"
      variant="outlined"
      :grid="true"
      dense
    />
  </mp-slide>
</template>

<script setup lang="ts">
import { ref } from "vue";

const tasks = ref([
  {
    label: "Анализ требований и ТЗ",
    start: "2025-11-01",
    end: "2025-11-02",
    color: "secondary",
  },
  {
    label: "Настройка проекта и инфраструктуры",
    start: "2025-11-02",
    end: "2025-11-03",
    color: "secondary",
  },
  {
    label: "Настройка базы данных",
    start: "2025-11-03",
    end: "2025-11-05",
    color: "tertiary",
  },
  {
    label: "Дизаайн Kanban компонентов",
    start: "2025-11-04",
    end: "2025-11-07",
    color: "secondary",
  },
  {
    label: "Настройка системы аутентификации",
    start: "2025-11-05",
    end: "2025-11-08",
    color: "tertiary",
  },
  {
    label: "API для пользователей и досок",
    start: "2025-11-08",
    end: "2025-11-11",
    color: "tertiary",
  },
  {
    label: "API для задач",
    start: "2025-11-11",
    end: "2025-11-14",
    color: "tertiary",
  },
  {
    label: "Разработка Kanban компонентов",
    start: "2025-11-07",
    end: "2025-11-12",
    color: "primary",
  },
  {
    label: "Реализация Drag & Drop",
    start: "2025-11-12",
    end: "2025-11-15",
    color: "primary",
  },
  {
    label: "Интеграция API с фронтендом",
    start: "2025-11-14",
    end: "2025-11-17",
    color: "primary",
  },
  {
    label: "Тестирование функциональности",
    start: "2025-11-17",
    end: "2025-11-19",
    color: "primary",
  },
  {
    label: "Исправление багов",
    start: "2025-11-19",
    end: "2025-11-20",
    color: "primary",
  },
  {
    label: "Адаптивный дизайн",
    start: "2025-11-15",
    end: "2025-11-18",
    color: "primary",
  },
  {
    label: "Реализация доступности",
    start: "2025-11-17",
    end: "2025-11-19",
    color: "primary",
  },
  {
    label: "Подготовка релиза",
    start: "2025-11-10",
    end: "2025-11-15",
    color: "secondary",
  },
  {
    label: "Подготовка документации",
    start: "2025-11-14",
    end: "2025-11-21",
    color: "secondary",
  },
]);
</script>

<style scoped></style>
