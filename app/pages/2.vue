<script setup lang="ts">
import type { TableColumn } from "../components/mp-table.vue";

interface ComparisonItem {
  aspect: string;
  jira: string;
  todo: string;
  teamly: string;
}

const columns: TableColumn[] = [
  { key: "aspect", label: "", width: "25%" },
  { key: "jira", label: "Jira", width: "25%", align: "center" },
  { key: "todo", label: "To-Do", width: "25%", align: "center" },
  { key: "teamly", label: "Teamly✨", width: "25%", align: "center" },
];

const items: ComparisonItem[] = [
  {
    aspect: "Порог входа",
    jira: "❌",
    todo: "✅",
    teamly: "✅",
  },
  {
    aspect: "Коллаборация",
    jira: "✅",
    todo: "❌",
    teamly: "✅",
  },
  {
    aspect: "Функциональность",
    jira: "✅",
    todo: "❌",
    teamly: "✅",
  },
  {
    aspect: "Цена",
    jira: "💲💲💲",
    todo: "💲",
    teamly: "💲",
  },
  {
    aspect: "Кастомизация",
    jira: "✅",
    todo: "❌",
    teamly: "✅",
  },
  {
    aspect: "Интеграции",
    jira: "🟨",
    todo: "❌",
    teamly: "✅",
  },
  {
    aspect: "Производительность",
    jira: "🟨",
    todo: "🟨",
    teamly: "✅",
  },
  {
    aspect: "Мобильная версия",
    jira: "❌",
    todo: "❌",
    teamly: "🟨",
  },
  {
    aspect: "Аналитика",
    jira: "✅",
    todo: "❌",
    teamly: "✅",
  },
  {
    aspect: "Поддержка",
    jira: "✅",
    todo: "❌",
    teamly: "✅",
  },
  {
    aspect: "Обучение",
    jira: "❌",
    todo: "✅",
    teamly: "✅",
  },
  {
    aspect: "Масштабируемость",
    jira: "✅",
    todo: "❌",
    teamly: "🟨",
  },
  {
    aspect: "UX/UI",
    jira: "🟨",
    todo: "🟨",
    teamly: "✅",
  },
  {
    aspect: "Доступность",
    jira: "✅",
    todo: "🟨",
    teamly: "✅",
  },
  {
    aspect: "Безопасность",
    jira: "✅",
    todo: "❌",
    teamly: "✅",
  },
];
</script>

<template>
  <mp-slide>
    <template #header>
      <h1>Сравнение решений</h1>
    </template>

    <div class="comparison-container">
      <mp-table
        :items="items"
        :columns="columns"
        variant="outlined"
        hover
        striped
        dense
        padding="s"
        body-text-size="title"
        header-text-size="title"
      >
        <template #cell-aspect="{ value }">
          <strong>{{ value }}</strong>
        </template>

        <template #cell-teamly="{ value }">
          <span>{{ value }}</span>
        </template>
      </mp-table>
    </div>
  </mp-slide>
</template>

<style scoped>
.comparison-container {
  display: flex;
  flex-direction: column;
  justify-content: center;
  height: 100%;
}
</style>
