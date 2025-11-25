<template>
  <mp-slide>
    <template #header>
      <h1>Проблематика: Хаос в управлении задачами</h1>
    </template>

    <div class="problem-page">
      <div class="content">
        <div class="problem">
          <h2>🎭 Проблема</h2>
          <p>
            Без эффективного управления задачи превращаются в хаос, вызывая
            стресс и конфликты. Традиционные методы недостаточно эффективны.
          </p>
          <p>
            <strong>Enterprise-решения (Jira)</strong> слишком сложны и избыточны
            для малых команд. Высокий порог входа.
          </p>
          <p>
            <strong>Простые To-Do листы</strong> не поддерживают коллаборацию,
            визуализацию процессов и управление правами доступа.
          </p>
        </div>

        <div class="solution">
          <h2>✨ Решение</h2>
          <p>
            <strong>Teamly</strong> — простая и интуитивная Kanban-доска.
            Проверенные методики в понятной обертке.
          </p>
          <p>
            <strong>Без сложности:</strong> визуализация задач, drag-and-drop,
            совместная работа. Никаких перегруженных интерфейсов.
          </p>
          <p>
            <strong>Масштабируемая архитектура</strong> на основе Nuxt Layers.
            Готова к расширению без переписывания ядра.
          </p>
          <p>
            <strong>Безопасность:</strong> ролевая модель доступа, stateful
            sessions, строгая изоляция данных.
          </p>
        </div>
      </div>

      <div class="image">
        <mp-image
          src="img/family-struggle.png"
          alt="Семейный хаос с задачами"
          fit="contain"
          :zoom="1.1"
        />
      </div>
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
definePageMeta({
  layout: "default",
});
</script>

<style scoped>
.problem-page {
  display: grid;
  grid-template-columns: 3fr 1fr;
  gap: var(--sp-xl);
  height: 100%;
  align-items: start;
}

.content {
  display: grid;
  gap: calc(var(--sp-l) * 4);
  max-width: 800px;
  margin-top: auto;
  margin-bottom: auto;
}

.problem,
.solution {
  display: grid;
  gap: var(--sp-m);
}

.problem h2 {
  color: var(--clr-error);
  margin: 0;
}

.solution h2 {
  color: var(--clr-success);
  margin: 0;
}

.problem p,
.solution p {
  margin: 0;
  line-height: 1.6;
}

.problem p strong,
.solution p strong {
  color: var(--clr-primary);
}

.image {
  display: grid;
  margin: 0;
  & picture {
    border-radius: var(--sp-l, 1rem);
  }
}
</style>
