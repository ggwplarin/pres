<template>
  <mp-slide>
    <template #header>
      <h1>Ролевая модель доступа (RBAC)</h1>
    </template>

    <div class="rbac-container">
      <div class="description">
        <h2>Система прав доступа</h2>
        <p>
          Права доступа жестко типизированы через enum BoardRole в Prisma.
          Каждая роль имеет четко определенные возможности.
        </p>
        <p>
          Строгая изоляция данных: пользователь видит только те доски, куда он
          имеет доступ.
        </p>
      </div>

      <div class="table-container">
        <mp-table
          :items="roleItems"
          :columns="columns"
          variant="outlined"
          hover
          striped
          dense
          padding="m"
          body-text-size="title"
          header-text-size="title"
        >
          <template #cell-role="{ value }">
            <strong>{{ value }}</strong>
          </template>
        </mp-table>
      </div>

      <div class="roles-description">
        <mp-bento-grid :cols="3" :rows="2" gap="m">
          <mp-bento-tile variant="outlined">
            <template #title>
              <h3>👑 Owner</h3>
            </template>
            <p>Полный доступ, удаление доски, управление приглашениями</p>
          </mp-bento-tile>

          <mp-bento-tile variant="outlined">
            <template #title>
              <h3>✏️ Editor</h3>
            </template>
            <p>Создание, редактирование контента, перемещение задач</p>
          </mp-bento-tile>

          <mp-bento-tile variant="outlined">
            <template #title>
              <h3>👁️ Viewer</h3>
            </template>
            <p>Только чтение (Read-only доступ)</p>
          </mp-bento-tile>
        </mp-bento-grid>
      </div>
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
import type { TableColumn } from "../components/mp-table.vue";

interface RoleItem {
  role: string;
  createTasks: string;
  deleteTasks: string;
  moveTasks: string;
  boardSettings: string;
}

const columns: TableColumn[] = [
  { key: "role", label: "Роль", width: "20%" },
  { key: "createTasks", label: "Создание задач", width: "20%", align: "center" },
  { key: "deleteTasks", label: "Удаление задач", width: "20%", align: "center" },
  { key: "moveTasks", label: "Перемещение", width: "20%", align: "center" },
  { key: "boardSettings", label: "Настройки доски", width: "20%", align: "center" },
];

const roleItems: RoleItem[] = [
  {
    role: "Owner",
    createTasks: "✅",
    deleteTasks: "✅",
    moveTasks: "✅",
    boardSettings: "✅",
  },
  {
    role: "Editor",
    createTasks: "✅",
    deleteTasks: "✅",
    moveTasks: "✅",
    boardSettings: "❌",
  },
  {
    role: "Viewer",
    createTasks: "❌",
    deleteTasks: "❌",
    moveTasks: "❌",
    boardSettings: "❌",
  },
];

definePageMeta({
  layout: "default",
});
</script>

<style scoped>
.rbac-container {
  height: 100%;
  display: grid;
  grid-template-rows: auto min-content 1fr;
  gap: calc(var(--sp-xl) * 2.5);
  align-content: start;
}

.description {
  display: grid;
  gap: var(--sp-s);
}

.description h2 {
  margin: 0;
  color: var(--clr-primary);
}

.description p {
  margin: 0;
  line-height: 1.5;
}

.table-container {
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.roles-description {
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.roles-description :deep(.teamly-bento-grid) {
  flex: 1;
  align-items: stretch;
}
</style>

