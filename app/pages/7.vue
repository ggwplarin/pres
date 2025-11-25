<template>
  <mp-slide>
    <template #header>
      <h1>Разработка MVP</h1>
    </template>
    <div class="container">
      <div class="image-container">
        <mp-image
          src="/img/roadmap.png"
          alt="Roadmap"
          fit="contain"
          :zoom="1.91"
        />
      </div>
      <div class="roadmap-container">
        <h2>Дорожная карта</h2>
        <mp-roadmap
          :items="roadmapItems"
          title-key="label"
          subtitle-key="description"
          date-key="date"
          status-key="status"
          color-key="color"
          direction="horizontal"
        />
      </div>
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
import { ref } from "vue";

const roadmapItems = ref([
  {
    label: "Проектирование",
    description: "Анализ требований, ТЗ, схема БД",
    date: "Ноябрь 1 - 5",
    status: "completed",
  },
  {
    label: "Бэкенд ядро",
    description: "Модели, Аутентификация, API задач",
    date: "Ноябрь 6 - 10",
    status: "completed",
  },
  {
    label: "UI и Frontend",
    description: "Компоненты, Kanban доска, Drag&Drop",
    date: "Ноябрь 11 - 15",
    status: "completed",
  },
  {
    label: "Интеграция и Тесты",
    description: "Связка API, E2E тесты, Багфикс",
    date: "Ноябрь 16 - 20",
    status: "completed",
  },
  {
    label: "Drag & Drop и интеграция",
    description: "Реализация перетаскивания, интеграция компонентов",
    date: "Ноябрь 12 - 16",
    status: "pending",
  },
  {
    label: "Функции задач и фильтры",
    description: "Расширенные возможности задач, система фильтрации",
    date: "Ноябрь 21 - 24",
    status: "pending",
  },
  {
    label: "Лендинг и мобильный дизайн",
    description: "Создание главной страницы, мобильная адаптация",
    date: "Ноябрь 25 - 30",
    status: "pending",
  },
  {
    label: "Тестирование и релиз",
    description: "Финальное тестирование, подготовка к запуску",
    date: "Ноябрь 30 - Декабрь 6",
    status: "pending",
  },
]);
</script>

<style scoped>
.container {
  display: grid;
  grid-template-rows: 36rem 1fr;
  gap: var(--sp-l);
}
.image-container {
  display: grid;
  margin: 0;
  & picture {
    border-radius: var(--sp-l, 1rem);
  }
}
.roadmap-container {
  display: grid;

  & h2 {
    margin: 0;
  }
}
</style>
