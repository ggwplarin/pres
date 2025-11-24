<template>
  <mp-slide>
    <template #header>
      <h1>Итоги разработки</h1>
    </template>

    <div class="results-container">
      <mp-bento-grid :cols="12" :rows="6" gap="m">
        <!-- Главный результат -->
        <mp-bento-tile variant="outlined" :col-span="12" :row-span="2">
          <template #title>
            <h2>✅ Разработано полнофункциональное веб-приложение (MVP)</h2>
          </template>
          <div class="main-result">
            <p>
              Создано современное SPA приложение для управления задачами с полным
              циклом разработки от проектирования до развертывания.
            </p>
          </div>
        </mp-bento-tile>

        <!-- Kanban логика -->
        <mp-bento-tile variant="outlined" :col-span="6" :row-span="2">
          <template #title>
            <h3>🎯 Kanban с оптимистичным UI</h3>
          </template>
          <p>
            Реализована сложная логика Kanban-доски с оптимистичными обновлениями
            интерфейса для мгновенной реакции пользователя.
          </p>
          <p>
            Drag-and-drop функциональность с автоматической синхронизацией
            состояния.
          </p>
        </mp-bento-tile>

        <!-- Безопасность -->
        <mp-bento-tile variant="outlined" :col-span="6" :row-span="2">
          <template #title>
            <h3>🔒 Безопасность данных</h3>
          </template>
          <p>
            Обеспечена безопасность данных и изоляция пользователей через
            ролевую модель доступа (RBAC).
          </p>
          <p>
            Stateful sessions, валидация через Zod, защита маршрутов через
            middleware.
          </p>
        </mp-bento-tile>

        <!-- Архитектура -->
        <mp-bento-tile variant="outlined" :col-span="6" :row-span="2">
          <template #title>
            <h3>🏗️ Модульная архитектура</h3>
          </template>
          <p>
            Архитектура на основе Nuxt Layers позволяет легко расширять
            функционал без переписывания ядра приложения.
          </p>
          <p>
            Слабое зацепление кода, изолированные слои, типобезопасность через
            TypeScript и Prisma.
          </p>
        </mp-bento-tile>

        <!-- Docker -->
        <mp-bento-tile variant="outlined" :col-span="6" :row-span="2">
          <template #title>
            <h3>🐳 Готово к развертыванию</h3>
          </template>
          <p>
            Приложение полностью контейнеризировано и готово к развертыванию
            через Docker Compose.
          </p>
          <p>
            Настроена инфраструктура с PostgreSQL, healthchecks, multi-stage
            сборка для production.
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
.results-container {
  height: 100%;
  display: flex;
  flex-direction: column;
  min-height: 0;
}

.results-container :deep(.teamly-bento-grid) {
  flex: 1;
  height: 100%;
}

.main-result {
  display: grid;
  gap: var(--sp-m);
}

.main-result p {
  margin: 0;
  line-height: 1.6;
  font-size: var(--ts-body);
}

h2 {
  margin: 0;
  color: var(--clr-primary);
}

h3 {
  margin: 0;
  color: var(--clr-primary);
}

p {
  margin: 0;
  line-height: 1.6;
}
</style>

