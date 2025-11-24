<template>
  <mp-slide>
    <template #header>
      <h1>Архитектура приложения</h1>
    </template>

    <div class="architecture-container">
      <div class="description">
        <h2>Модульный подход</h2>
        <p>
          Вместо монолита используется архитектура Nuxt Layers для слабого
          зацепления кода и модульности.
        </p>
        <p>
          Каждый слой изолирован и может быть легко расширен или заменен без
          влияния на другие части приложения.
        </p>
      </div>

      <mp-bento-grid :cols="12" :rows="4" gap="m">
      <!-- <mp-bento-tile pure :row-span="1" :col-span="6"></mp-bento-tile>
      <mp-bento-tile pure :row-span="1" :col-span="6"></mp-bento-tile> -->
        <!-- Base App -->
        <mp-bento-tile variant="outlined" :col-span="12" :row-span="1">
          <template #title>
            <h3>Base App</h3>
          </template>
          <p>Основное приложение Nuxt с конфигурацией и роутингом</p>
        </mp-bento-tile>

        <!-- Core Layer -->
        <mp-bento-tile variant="outlined" :col-span="12" :row-span="1">
          <template #title>
            <h3>📦 layers/core</h3>
          </template>
          <p>
            Базовые UI-атомы, глобальные стили и утилиты. Фундамент для всех
            остальных слоев.
          </p>
        </mp-bento-tile>

        <!-- Auth Layer -->
        <mp-bento-tile variant="outlined" :col-span="6" :row-span="1">
          <template #title>
            <h3>🔒 layers/auth</h3>
          </template>
          <p>Изолированная логика регистрации и сессий. Аутентификация и авторизация.</p>
        </mp-bento-tile>

        <!-- Board Layer -->
        <mp-bento-tile variant="outlined" :col-span="6" :row-span="1">
          <template #title>
            <h3>📋 layers/board</h3>
          </template>
          <p>Бизнес-логика Kanban-доски. Управление досками и участниками.</p>
        </mp-bento-tile>

        <!-- Tasks Layer -->
        <mp-bento-tile variant="outlined" :col-span="12" :row-span="1">
          <template #title>
            <h3>✅ layers/tasks</h3>
          </template>
          <p>
            Управление сущностями задач. CRUD операции, фильтрация, сортировка.
          </p>
        </mp-bento-tile>
      </mp-bento-grid>
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
definePageMeta({
  layout: "default",
});
</script>

<style scoped>
:deep(.bento-tile-content) {
  display: flex;
  flex-direction: column;
  justify-content: flex-end;
}

:deep(.bento-tile-content p) {
    margin: 0;
}

.architecture-container {
  height: 100%;
  display: grid;
  grid-template-rows: auto 1fr;
  gap: var(--sp-xl);
}

.description {
  display: grid;
  gap: var(--sp-m);
}

.description h2 {
  margin: 0;
  color: var(--clr-primary);
}

.description p {
  margin: 0;
  line-height: 1.6;
}
</style>

