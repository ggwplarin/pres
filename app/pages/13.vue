<template>
  <mp-slide>
    <template #header>
      <h1>Работа с цветовой палитрой</h1>
    </template>

    <div class="slide-content">
      <div class="description">
        <h2>Material Design 3</h2>
        <div class="image">
          <mp-image
            src="img/material-design-3.png"
            alt="Material Design 3"
            fit="contain"
            :zoom="1.1"
          />
        </div>
        <h3>Описание системы</h3>
        <p>
          Современный подход к цветовой системе использует LCH цветовое
          пространство с динамическими переменными для создания адаптивных тем.
        </p>
        <p>
          Цвета определяются через базовые параметры (тон, насыщенность,
          яркость) и автоматически генерируют всю палитру: primary, secondary,
          tertiary, нейтральные тона и специальные цвета (error, success).
        </p>
        <h3>Использование в приложении</h3>
        <p>
          Такой подход обеспечивает консистентность и позволяет легко менять
          тему приложения через изменение нескольких базовых переменных.
        </p>
      </div>

      <div class="code-examples">
        <mp-codeblock
          :code="colorDefinitions"
          language="css"
          filename="public/color.css"
          show-line-numbers
          :highlight-lines="[1, 2, 11]"
        />

        <mp-codeblock
          :code="usageExample"
          language="css"
          filename="public/global.css"
          show-line-numbers
          :highlight-lines="[3]"
        />
      </div>
    </div>
  </mp-slide>
</template>

<script setup lang="ts">
const colorDefinitions = `:root[data-theme="light"],
.theme-light {
  --clr-primary: lch(60% var(--p-c) var(--p-h)); /* T40 */
  --clr-primary-vibrant: lch(60% calc(var(--p-c) * var(--v-b)) var(--p-h));
  --clr-on-primary: lch(12% var(--p-c) var(--p-h)); /* T10 */
  --clr-primary-container: lch(92% calc(var(--p-c) * 0.6) var(--p-h)); /* T90 */
  --clr-primary-container-vibrant: lch(92% calc(var(--p-c) * 0.9) var(--p-h));
  --clr-on-primary-container: lch(25% var(--p-c) var(--p-h)); /* T30 */

  /* Base background / surface */
  --clr-background: lch(98% var(--n-c) var(--p-h)); /* T98 */
  --clr-on-background: lch(10% var(--n-c) var(--p-h)); /* T10 */
  --clr-surface: lch(98% var(--n-c) var(--p-h)); /* T98 */
  --clr-surface-dim: lch(92% var(--n-c) var(--p-h)); /* T90 */
  --clr-surface-bright: lch(100% var(--n-c) var(--p-h)); /* T100 */
  ...
}`;

const usageExample = `input,
textarea {
  background-color: var(--clr-background);
  border: var(--sp-outline) solid var(--clr-outline);
  padding: var(--sp-s);
  border-radius: var(--sp-s);
  color: var(--clr-on-surface);
}`;
</script>

<style scoped>
.slide-content {
  height: 100%;
  display: grid;
  grid-template-columns: 1fr 1fr auto;
  gap: var(--sp-xl);
  align-items: start;
}

.image {
  display: grid;
  margin: 0;
  & picture {
    border-radius: var(--sp-l, 1rem);
  }
}

.code-examples {
  display: grid;
  grid-template-rows: 1fr 1fr;
  gap: var(--sp-l);
  height: 100%;
}
</style>
