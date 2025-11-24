<template>
  <mp-slide>
    <template #header>
      <h1>Технический стек</h1>
    </template>
    <mp-bento-grid :cols="8" :rows="5">
      <!-- Основные фреймворки и языки -->
      <mp-bento-tile variant="outlined" :row-span="2" :col-span="2">
        <template #title>
          <h3>TypeScript</h3>
        </template>
        <template #image>
          <mp-image
            src="img/typescript-logo.svg"
            alt="TypeScript"
            fit="contain"
            :zoom="0.8"
          />
        </template>
        <p>
          Строго типизированный язык, расширяющий JavaScript статическими типами
        </p>
        <p>
          Обеспечивает безопасность кода и снижает количество runtime ошибок в
          production
        </p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :row-span="2" :col-span="2">
        <template #title>
          <h3>Vue</h3>
        </template>
        <template #image>
          <mp-image
            :width="1001"
            :height="1001"
            src="img/vue-logo.svg"
            alt="Vue.js"
            fit="contain"
            aspectRatio="1/1"
          />
        </template>
        <p>Прогрессивный фреймворк для создания пользовательских интерфейсов</p>
        <p>
          Проверенный и надежный инструмент для создания сложных интерфейсов с
          отличным DX
        </p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :row-span="2" :col-span="2">
        <template #title>
          <h3>Nuxt</h3>
        </template>
        <template #image>
          <mp-image src="img/nuxt-logo.svg" alt="Nuxt.js" fit="contain" />
        </template>
        <p>
          Мета-фреймворк на базе Vue.js для создания универсальных приложений
        </p>
        <p>
          Full-stack Vue фреймворк с SSR/SSG для оптимальной производительности
          и SEO
        </p>
      </mp-bento-tile>

      <!-- State Management -->
      <mp-bento-tile variant="outlined" :row-span="2">
        <template #title>
          <h3>Pinia</h3>
        </template>
        <template #image>
          <mp-image
            src="img/pinia-logo.svg"
            alt="Pinia"
            fit="contain"
            aspectRatio="1/1"
          />
        </template>
        <p>Официальный state manager для Vue с отличной DX</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :row-span="2">
        <template #title>
          <h3>Pinia Colada</h3>
        </template>
        <template #image>
          <mp-image
            src="img/colada-logo.svg"
            alt="Pinia Colada"
            fit="fill"
            aspectRatio="1/1"
          />
        </template>
        <p>Автоматизирует API запросы и кеширование данных</p>
      </mp-bento-tile>

      <!-- Backend и Database -->
      <mp-bento-tile variant="outlined" :row-span="2" :col-span="3">
        <template #title>
          <h3>Prisma + Accelerate</h3>
        </template>
        <template #image>
          <mp-image
            src="img/prisma-logo.svg"
            alt="Prisma"
            fit="contain"
            :zoom="0.8"
          />
        </template>
        <p>ORM с type-safe запросами к базе данных и глобальным кешированием</p>
        <p>Выбранa за безопасность типов и производительность</p>
        <p>Используется Accelerate для ускорения запросов</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :row-span="1">
        <template #title>
          <h3>Lucia Auth</h3>
        </template>
        <p>Безопасная и гибкая система авторизации</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :row-span="2">
        <template #title>
          <h3>date-fns</h3>
        </template>
        <template #image>
          <mp-image src="img/date-fns-logo.svg" alt="date-fns" fit="contain" />
        </template>
        <p>Модульная библиотека для работы с датами</p>
        <p>Надежная манипуляция датами без зависимостей</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :row-span="2" :col-span="2">
        <template #title>
          <h3>vite</h3>
        </template>
        <template #image>
          <mp-image
            src="img/vite-logo.svg"
            alt="Vite"
            fit="contain"
            :zoom="0.8"
          />
        </template>
        <p>Современный сборщик с молниеносным HMR и оптимизированной сборкой</p>
        <p>Обеспечивает быструю разработку и малый размер бандла</p>
      </mp-bento-tile>
      <!-- UI и UX -->

      <mp-bento-tile variant="outlined">
        <template #title>
          <h3>@vueuse/core</h3>
        </template>
        <p>Коллекция composables для common Vue паттернов</p>
        <p>Ускоряет разработку и повышает качество кода</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :row-span="1">
        <template #title>
          <h3>QRCode.vue</h3>
        </template>
        <p>Генерация QR кодов для Vue компонентов</p>
        <p>Простая интеграция QR кодов в интерфейс</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :row-span="1">
        <template #title>
          <h3>@nuxt/image</h3>
        </template>
        <p>Оптимизация изображений с автоматическим resizing и форматами</p>
        <p>Улучшает производительность загрузки страниц</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :col-span="2">
        <template #title>
          <h3>Nano ID</h3>
        </template>
        <template #image>
          <mp-image src="img/nano-id-logo.svg" alt="Nano ID" fit="contain" />
        </template>
        <p>Генерация уникальных идентификаторов</p>
        <p>Быстрый и безопасный генератор ID</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :col-span="3">
        <template #title>
          <h3>@formkit/drag-and-drop</h3>
        </template>
        <template #image>
          <mp-image
            src="img/formkit-dnd-logo.svg"
            alt="Formkit DND"
            fit="contain"
          />
        </template>
        <p>Легковесная библиотека для перетаскивания элементов</p>
        <p>Минимальный вес бандла и простота использования</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :col-span="1">
        <template #title>
          <h3>@nuxt/fonts</h3>
        </template>
        <p>Автоматическая оптимизация загрузки шрифтов</p>
        <p>Улучшает производительность и SEO</p>
      </mp-bento-tile>

      <mp-bento-tile variant="outlined" :col-span="2">
        <template #title>
          <h3>Zod</h3>
        </template>
        <template #image>
          <mp-image src="img/zod-logo.svg" alt="Zod" fit="contain" />
        </template>
        <p>Type-safe валидация данных и схемы</p>
      </mp-bento-tile>
    </mp-bento-grid>
  </mp-slide>
</template>
