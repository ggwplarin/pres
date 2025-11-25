<template>
  <mp-slide>
    <template #header>
      <h1>Модель данных Kanban</h1>
    </template>

    <div class="kanban-model-container">
      <div class="description">
        <h2>Структура данных задач</h2>
        <p>
          Статусы задач зафиксированы: <strong>todo</strong>,
          <strong>in_progress</strong>, <strong>done</strong>.
        </p>
        <p>
          Это решение принято для MVP, чтобы избежать сложности управления
          динамическими колонками.
        </p>
        <p>
          Поле <code>order_index</code> зарезервировано для пользовательской
          сортировки (vNext).
        </p>
      </div>

      <div class="code-container">
        <mp-codeblock
          :code="taskModelCode"
          language="prisma"
          filename="prisma/schema.prisma"
          show-line-numbers
          :highlight-lines="[1, 13]"
        />
      </div>
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
const taskModelCode = `enum TaskStatusEnum {
  todo
  in_progress
  done
}

model Task {
  id          String          @id @default(uuid())
  board_id    String
  board       Board           @relation(fields: [board_id], references: [id], onDelete: Cascade)
  title       String
  description String?
  status      TaskStatusEnum  @default(todo)
  priority    TaskPriority    @default(medium)
  order_index Int?
  due_date    DateTime?
  created_at  DateTime        @default(now())
  updated_at  DateTime        @updatedAt

  assignees   TaskAssignee[]
}`;

definePageMeta({
  layout: "default",
});
</script>

<style scoped>
.kanban-model-container {
  height: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--sp-xl);
  align-items: center;
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

.description code {
  background-color: var(--clr-surface-variant);
  padding: 0.125rem 0.375rem;
  border-radius: var(--cr-s);
  font-family: "Fira Code", monospace;
  font-size: 0.9em;
  color: var(--clr-primary);
}
</style>

