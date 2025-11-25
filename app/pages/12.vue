<template>
  <mp-slide>
    <template #header>
      <h1>Создание доски</h1>
    </template>

    <div class="code-container">
      <div>
        <h2>Логика создания новой доски</h2>
        <p>
          API endpoint для создания новой Kanban доски с валидацией данных,
          проверкой авторизации и автоматическим добавлением создателя как
          владельца.
        </p>
        <p>
          Использует Zod для валидации входных данных и Prisma для работы с БД.
          Поддерживает приватность доски и описание.
        </p>
        <p>
          При успешном создании доски владелец автоматически добавляется в
          список участников с ролью "owner".
        </p>
      </div>
      <mp-codeblock
        :code="createBoardCode"
        language="typescript"
        filename="server/api/board/index.post.ts"
        show-line-numbers
        :highlight-lines="[2, 3, 8, 16]"
      />
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
const createBoardCode = `
export default defineEventHandler(async (event) => {
  const prisma = await usePrisma();
  const user = await getUserFromSession(event);

  if (!user) return sendError(event, createError({ statusCode: 401 }));

  const body = await readBody(event);
  const parsed = BoardCreateSchema.safeParse(body);

  if (!parsed.success)
    return sendError(
      event,
      createError({ statusCode: 422, statusMessage: "Неверные данные" })
    );

  return await prisma.board.create({
    data: {
      ...parsed.data,
      owner_user_id: user.id,
      participants: {
        create: [
          {
            user_id: user.id,
            role: "owner",
          },
        ],
      },
    },
  });
});`;
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
