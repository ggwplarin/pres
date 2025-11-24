<template>
  <mp-slide>
    <template #header>
      <h1>Авторизация и защита маршрутов</h1>
    </template>

    <div class="code-container">
      <div>
        <h2>Пример работы auth middleware</h2>
        <p>
          Middleware проверяет авторизацию пользователя и защищает приватные
          маршруты.
        </p>
        <p>
          Если пользователь не авторизован и пытается попасть на приватный
          маршрут, он будет перенаправлен на страницу авторизации.
        </p>
        <p>
          Если пользователь авторизован и пытается попасть на страницу
          авторизации, он будет перенаправлен на главную страницу.
        </p>
      </div>
      <mp-codeblock
        :code="authCode"
        language="typescript"
        filename="layers/auth/middleware/auth.global.ts"
        show-line-numbers
        :highlight-lines="[5, 10, 15]"
      />
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
const authCode = `export default defineNuxtRouteMiddleware(async (to) => {
  const userStore = useUserStore();

  // Проверка авторизации
  if (userStore.isLoggedIn && to.path === '/login') {
    return navigateTo('/');
  }

  // Защита приватных роутов
  if (!userStore.isLoggedIn && to.meta.requiresAuth) {
    return navigateTo('/login');
  }

  // Проверка прав доступа
  if (to.meta.role && userStore.role !== to.meta.role) {
    return navigateTo('/403');
  }
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
