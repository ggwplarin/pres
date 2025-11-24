<script setup lang="ts">
import { computed } from "vue";
import type { ColorName, SpacingName, VariantName } from "../types/tokens";

/**
 * Резолвит CSS‑переменную для цвета плитки на основе color/variant.
 * - filled: основной цвет, например, --clr-primary
 * - tonal: контейнерный цвет, например, --clr-primary-container
 * - outlined: прозрачный фон (transparent), border задаётся отдельно
 */
function resolveColor(
  color: ColorName | (string & {}) = "surface",
  variant: VariantName = "filled"
): string {
  switch (variant) {
    case "filled":
      return `var(--clr-${color})`;
    case "tonal":
      return `var(--clr-${color}-container)`;
    case "outlined":
      return `transparent`;
    default:
      return `var(--clr-${color})`;
  }
}

// Вынести resolveSpacing, если он у тебя есть!
import { resolveSpacing } from "../utils/spacing";

const props = defineProps<{
  rowSpan?: number;
  colSpan?: number;
  color?: ColorName | (string & {}); // Teamly color token, например "surface", "primary"
  variant?: VariantName; // "filled", "outlined" и т.п.
  pure?: boolean;
  as?: "div" | "section" | "article" | (string & {});
  padding?: SpacingName | (string & {}) | number; // spacing-token ("m", "l" и т.д.)
}>();

const tileColor = computed(() =>
  props.pure
    ? undefined
    : resolveColor(props.color ?? "surface", props.variant ?? "filled")
);

const style = computed(() => ({
  "grid-row": `span ${props.rowSpan ?? 1}`,
  "grid-column": `span ${props.colSpan ?? 1}`,
  background: tileColor.value,
  padding: props.padding ? resolveSpacing(props.padding) : "var(--sp-l)",
}));
</script>

<template>
  <component
    :is="props.as ?? 'div'"
    class="bento-tile-container"
    :class="{
      'bento-tile--pure': props.pure,
      [`bento-tile--${props.variant}`]: props.variant,
    }"
    :style="style"
    :data-variant="props.variant ?? 'filled'"
    :data-color="props.color ?? 'surface'"
  >
    <div class="bento-tile">
      <div class="bento-tile-title">
        <slot name="title" />
      </div>
      <div class="bento-tile-image">
        <slot name="image" />
      </div>
      <div class="bento-tile-content">
        <slot />
      </div>
    </div>
  </component>
</template>

<style>
.bento-tile-container {
  container-type: size;
  min-width: 0;
  min-height: 0;
  box-sizing: border-box;
  background: var(--clr-surface);
  padding: var(--sp-l, 1.618rem);
  border-radius: var(--sp-l, 1rem);
  display: grid;
}

.bento-tile {
  display: grid;
  gap: var(--sp-m);
  overflow: hidden;
  :first-child:is(h1, h2, h3, h4, h5, h6) {
    margin-block: 0;
    color: var(--clr-on-surface-variant);
  }
  & picture {
    border-radius: var(--sp-s, 1rem);
  }
}

@container (aspect-ratio > 1) {
  .bento-tile {
    /* Landscape layout */
    grid-template-areas: "title" "content";
    grid-template-rows: auto 1fr;

    &:has(> .bento-tile-image > *) {
      grid-template-areas: "title image" "content image";
      grid-template-columns: auto minmax(50%, 1fr);
    }
  }
}

@container (aspect-ratio <= 1) {
  .bento-tile {
    /* Portrait layout */
    grid-template-areas: "title" "image" "content";
    grid-template-columns: 1fr;
    grid-template-rows: auto 1fr;
    &:has(> .bento-tile-content > *) {
      grid-template-rows: auto 1fr auto;
    }
  }
}

.bento-tile--pure {
  background: none !important;
  box-shadow: none !important;
  padding: 0 !important;
}
.bento-tile--outlined {
  border: 1px solid var(--clr-outline);
}
.bento-tile-title {
  grid-area: title;
  display: grid;
  &:not(:has(*)) {
    display: none;
  }
}
.bento-tile-image {
  grid-area: image;
  height: 100%;
  width: 100%;
  display: grid;
  min-height: 0;
  min-width: 0;
  &:not(:has(*)) {
    display: none;
  }
}
.bento-tile-content {
  grid-area: content;
  &:not(:has(*)) {
    display: none;
  }
}
</style>
