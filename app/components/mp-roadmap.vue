<script setup lang="ts" generic="T extends Record<string, any>">
import { computed } from "vue";
import type { ColorName } from "../types/tokens";

const props = withDefaults(
  defineProps<{
    items: T[];
    titleKey?: string;
    subtitleKey?: string;
    dateKey?: string;
    statusKey?: string; // 'completed' | 'current' | 'pending' | 'future'
    colorKey?: string;
    direction?: "vertical" | "horizontal";
  }>(),
  {
    titleKey: "title",
    subtitleKey: "subtitle",
    dateKey: "date",
    statusKey: "status",
    colorKey: "color",
    direction: "vertical",
  }
);

const steps = computed(() => {
  return props.items.map((item) => ({
    title: item[props.titleKey],
    subtitle: item[props.subtitleKey],
    date: item[props.dateKey],
    status: item[props.statusKey] || "pending",
    color: item[props.colorKey],
    original: item,
  }));
});

function getStatusColor(status: string, itemColor?: string) {
  if (itemColor) return `var(--clr-${itemColor})`;

  switch (status) {
    case "completed":
      return "var(--clr-primary)";
    case "current":
      return "var(--clr-primary)";
    case "pending":
    case "future":
    default:
      return "var(--clr-outline)";
  }
}
</script>

<template>
  <ol class="mp-roadmap" :class="[`mp-roadmap--${direction}`]">
    <li
      v-for="(step, index) in steps"
      :key="index"
      class="mp-roadmap-step"
      :class="[`mp-roadmap-step--${step.status}`]"
      :style="{ '--step-color': getStatusColor(step.status, step.color) }"
    >
      <!-- Connector Line -->
      <div class="mp-roadmap-line" v-if="index !== steps.length - 1"></div>

      <!-- Marker -->
      <div class="mp-roadmap-marker">
        <slot
          name="marker"
          :item="step.original"
          :status="step.status"
          :index="index"
        >
          <div class="mp-roadmap-dot"></div>
        </slot>
      </div>

      <!-- Content -->
      <div class="mp-roadmap-content">
        <div class="mp-roadmap-header">
          <span class="mp-roadmap-date" v-if="step.date">{{ step.date }}</span>
          <h3 class="mp-roadmap-title">
            <slot name="title" :item="step.original">{{ step.title }}</slot>
          </h3>
        </div>
        <div class="mp-roadmap-body" v-if="step.subtitle || $slots.default">
          <p class="mp-roadmap-subtitle" v-if="step.subtitle">
            <slot name="subtitle" :item="step.original">{{
              step.subtitle
            }}</slot>
          </p>
          <slot :item="step.original" />
        </div>
      </div>
    </li>
  </ol>
</template>

<style scoped>
.mp-roadmap {
  display: flex;
  gap: var(--sp-l);
  width: 100%;
  height: 100%;
  list-style: none;
  padding: 0;
  margin: 0;
}

/* Vertical Layout */
.mp-roadmap--vertical {
  flex-direction: column;
  gap: 0; /* Gap handled by grid/padding in steps */
}

.mp-roadmap-step {
  display: grid;
  grid-template-columns: auto 1fr;
  gap: var(--sp-m);
  position: relative;
  padding-bottom: var(--sp-l);
}

.mp-roadmap-step:last-child {
  padding-bottom: 0;
}

/* Marker Area */
.mp-roadmap-marker {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: var(--sp-l);
  position: relative;
  z-index: 2;
}

.mp-roadmap-dot {
  width: 1rem;
  height: 1rem;
  border-radius: 50%;
  background-color: var(--clr-surface);
  border: 2px solid var(--step-color);
  transition: all 0.3s ease;
}

.mp-roadmap-step--completed .mp-roadmap-dot {
  background-color: var(--step-color);
}

.mp-roadmap-step--current .mp-roadmap-dot {
  background-color: var(--step-color);
  box-shadow: 0 0 0 4px color-mix(in srgb, var(--step-color), transparent 80%);
  animation: pulse 2s infinite;
}

@keyframes pulse {
  0% {
    box-shadow: 0 0 0 0px color-mix(in srgb, var(--step-color), transparent 40%);
  }
  100% {
    box-shadow: 0 0 0 8px
      color-mix(in srgb, var(--step-color), transparent 100%);
  }
}

/* Line */
.mp-roadmap-line {
  position: absolute;
  top: 1rem; /* Start below the dot */
  bottom: 0;
  left: calc(var(--sp-l) / 2 - 1px); /* Center in marker column */
  width: 2px;
  background-color: var(--clr-outline);
  z-index: 1;
}

.mp-roadmap-step--completed .mp-roadmap-line {
  background-color: var(--step-color);
}

/* Content */
.mp-roadmap-content {
  display: flex;
  flex-direction: column;
  gap: var(--sp-xs);
  padding-top: calc(
    1rem / 2 - var(--ts-title) / 2
  ); /* Align with dot vertically roughly */
}

.mp-roadmap-header {
  display: flex;
  flex-wrap: wrap;
  align-items: baseline;
  gap: var(--sp-s);
}

.mp-roadmap-date {
  font-size: var(--ts-label);
  color: var(--clr-primary);
  font-family: "Roboto Mono", monospace;
  font-weight: 500;
}

.mp-roadmap-title {
  margin: 0;
  font-size: var(--ts-title);
  color: var(--clr-on-surface);
  line-height: 1.2;
}

.mp-roadmap-subtitle {
  margin: 0;
  font-size: var(--ts-body);
  color: var(--clr-on-surface-variant);
}

/* Horizontal Layout (Optional, basic support) */
.mp-roadmap--horizontal {
  flex-direction: row;
  overflow-x: auto;
  padding-bottom: var(--sp-m);
}

.mp-roadmap--horizontal .mp-roadmap-step {
  display: flex;
  flex-direction: column;
  min-width: 200px;
  gap: var(--sp-s);
  padding-bottom: 0;
  padding-right: var(--sp-l);
}

.mp-roadmap--horizontal .mp-roadmap-marker {
  flex-direction: row;
  align-items: center;
  width: auto;
  height: var(--sp-l);
}

.mp-roadmap--horizontal .mp-roadmap-line {
  top: calc(var(--sp-l) / 2 - 1px);
  left: 1rem; /* Start after dot */
  right: -1.75rem; /* Extend to right */
  bottom: auto;
  width: auto;
  height: 2px;
}
.mp-roadmap--horizontal .mp-roadmap-content {
  padding-top: 0;
}
</style>
