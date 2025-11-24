<template>
  <mp-slide>
    <template #header>
      <h1>Схема данных</h1>
    </template>

    <div class="code-container">
      <div>
        <h2>Модель Board</h2>
        <p>
          Основная модель доски Kanban с отношениями к пользователям, задачам и
          участникам.
        </p>
        <p>
          Поддерживает приватность (visibility), архивацию и каскадное удаление
          зависимых данных.
        </p>
        <p>Связи с пользователями, задачами, приглашениями и чатами.</p>
      </div>
      <mp-codeblock
        :code="schemaCode"
        language="prisma"
        filename="prisma/schema.prisma"
        show-line-numbers
        :highlight-lines="[2, 3, 4, 7, 8]"
      />
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
const schemaCode = `model Board {
  id              String             @id @default(uuid())
  owner_user_id   String
  owner           User
  title           String
  description     String?
  visibility      BoardVisibility
  is_archived     Boolean            @default(false)
  deleted_at      DateTime?
  created_at      DateTime           @default(now())

  // Relations
  participants    BoardParticipant[]
  tasks           Task[]
  invitationLinks InvitationLink[]
  Conversation    Conversation[]
}`;
</script>

<style scoped>
.code-container {
  height: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: var(--sp-xl);
  align-items: center;
}

.code-container h2 {
  margin: 0;
  color: var(--clr-primary);
}
</style>
