<script setup lang="ts" generic="T extends Record<string, any>">
import { computed } from "vue";
import { resolveSpacing } from "../utils/spacing";
import type { ColorName, SpacingName } from "../types/tokens";

/**
 * Props definition
 */
const props = defineProps<{
  items: T[];
  /** Field key for the start date (default: 'start') */
  startKey?: keyof T;
  /** Field key for the end date (default: 'end') */
  endKey?: keyof T;
  /** Field key for the label (default: 'label') */
  labelKey?: keyof T;
  /** Field key for the color (default: 'color') */
  colorKey?: keyof T;

  /** Explicit start date for the chart */
  minDate?: string | Date;
  /** Explicit end date for the chart */
  maxDate?: string | Date;

  /** Visual variant */
  variant?: "default" | "outlined" | "filled";
  /** Compact mode */
  dense?: boolean;
  /** Custom padding */
  padding?: SpacingName | string | number;

  /** Show grid lines */
  grid?: boolean;
}>();

/**
 * Type definitions and helpers
 */
const getStart = (item: T): number => {
  const key = props.startKey ?? "start";
  return new Date(item[key]).getTime();
};

const getEnd = (item: T): number => {
  const key = props.endKey ?? "end";
  return new Date(item[key]).getTime();
};

const getLabel = (item: T): string => {
  const key = props.labelKey ?? "label";
  return String(item[key] ?? "");
};

const getColor = (item: T): string | undefined => {
  const key = props.colorKey ?? "color";
  return item[key];
};

// Resolve CSS color variable
function resolveColorStyle(color?: string): string | undefined {
  if (!color) return undefined;
  if (color.startsWith("--")) return `var(${color})`;
  if (
    ["primary", "secondary", "tertiary", "error", "surface"].some((c) =>
      color.includes(c)
    )
  ) {
    return `var(--clr-${color})`;
  }
  return color;
}

/**
 * Timeline Calculation
 */
const timeline = computed(() => {
  let min = props.minDate ? new Date(props.minDate).getTime() : Infinity;
  let max = props.maxDate ? new Date(props.maxDate).getTime() : -Infinity;

  if (props.items.length === 0 && (min === Infinity || max === -Infinity)) {
    const now = Date.now();
    return {
      min: now,
      max: now + 24 * 60 * 60 * 1000,
      duration: 24 * 60 * 60 * 1000,
    };
  }

  if (!props.minDate || !props.maxDate) {
    props.items.forEach((item) => {
      const s = getStart(item);
      const e = getEnd(item);
      if (!props.minDate && s < min) min = s;
      if (!props.maxDate && e > max) max = e;
    });
  }

  // Add some buffer if calculated automatically
  if (!props.minDate) min -= 24 * 60 * 60 * 1000 * 2; // -2 days
  if (!props.maxDate) max += 24 * 60 * 60 * 1000 * 2; // +2 days

  return { min, max, duration: max - min };
});

const getItemStyle = (item: T) => {
  const start = getStart(item);
  const end = getEnd(item);
  const { min, duration } = timeline.value;

  const left = ((start - min) / duration) * 100;
  const width = ((end - start) / duration) * 100;
  const color = resolveColorStyle(getColor(item));

  return {
    left: `${Math.max(0, left)}%`,
    width: `${Math.max(0.5, width)}%`, // At least visible
    backgroundColor: color || `var(--clr-primary)`,
  };
};

/**
 * Grid / Ticks Calculation (Months)
 */
const ticks = computed(() => {
  const { min, max, duration } = timeline.value;
  const result = [];

  // Heuristic: if less than 60 days, show weeks. Else show months.
  const isCompact = duration < 60 * 24 * 60 * 60 * 1000;

  if (isCompact) {
    // Weekly ticks
    let current = new Date(min);
    // Adjust to start of week (Monday)
    const day = current.getDay();
    const diff = current.getDate() - day + (day === 0 ? -6 : 1);
    current.setDate(diff);

    while (current.getTime() <= max) {
      const time = current.getTime();
      if (time >= min - 7 * 24 * 3600 * 1000) {
        // Allow partial week visibility
        const left = ((time - min) / duration) * 100;
        if (left <= 100) {
          result.push({
            left: `${left}%`,
            label: current.toLocaleDateString("ru-RU", {
              day: "numeric",
              month: "short",
            }),
            key: time,
          });
        }
      }
      current.setDate(current.getDate() + 7);
    }
  } else {
    // Monthly ticks
    const startDate = new Date(min);
    let current = new Date(startDate.getFullYear(), startDate.getMonth(), 1);

    while (current.getTime() <= max) {
      const time = current.getTime();
      if (time >= min) {
        const left = ((time - min) / duration) * 100;
        if (left <= 100) {
          result.push({
            left: `${left}%`,
            label: current.toLocaleDateString("ru-RU", {
              month: "short",
              year: "2-digit",
            }),
            key: time,
          });
        }
      }
      current.setMonth(current.getMonth() + 1);
    }
  }

  return result;
});

/**
 * Styling classes
 */
const containerClasses = computed(() => ({
  "mp-gantt--outlined": props.variant === "outlined",
  "mp-gantt--filled": props.variant === "filled",
  "mp-gantt--dense": props.dense,
  "mp-gantt--grid": props.grid !== false, // Default true-ish
}));

const customPadding = computed(() => {
  return props.padding ? resolveSpacing(props.padding) : null;
});
</script>

<template>
  <div
    class="mp-gantt-container"
    :class="containerClasses"
    :style="customPadding ? { padding: customPadding } : {}"
  >
    <!-- Header Row -->
    <div class="mp-gantt-row mp-gantt-header">
      <div class="mp-gantt-label-col">
        <slot name="header-label">Задача</slot>
      </div>
      <div class="mp-gantt-track-col">
        <div
          v-for="tick in ticks"
          :key="tick.key"
          class="mp-gantt-tick"
          :style="{ left: tick.left }"
        >
          {{ tick.label }}
        </div>
      </div>
    </div>

    <!-- Item Rows -->
    <div v-for="(item, index) in items" :key="index" class="mp-gantt-row">
      <div class="mp-gantt-label-col">
        <slot name="label" :item="item">
          {{ getLabel(item) }}
        </slot>
      </div>

      <div class="mp-gantt-track-col">
        <!-- Grid lines background -->
        <div v-if="grid" class="mp-gantt-grid-lines">
          <div
            v-for="tick in ticks"
            :key="tick.key"
            class="mp-gantt-grid-line"
            :style="{ left: tick.left }"
          ></div>
        </div>

        <!-- The Bar -->
        <div class="mp-gantt-bar-container" :style="getItemStyle(item)">
          <slot name="bar" :item="item">
            <div class="mp-gantt-bar"></div>
          </slot>
        </div>
      </div>
    </div>

    <!-- Empty State -->
    <div v-if="items.length === 0" class="mp-gantt-empty">
      <slot name="empty">Нет данных для отображения</slot>
    </div>
  </div>
</template>

<style scoped>
.mp-gantt-container {
  width: 100%;
  display: flex;
  flex-direction: column;
  background-color: var(--clr-background);
  border-radius: var(--sp-m);
  container-type: inline-size;
  overflow-x: auto;
  height: fit-content;
}

.mp-gantt--outlined {
  border: 1px solid var(--clr-outline);
}

.mp-gantt--filled {
  background-color: var(--clr-surface-container);
}

.mp-gantt-row {
  display: grid;
  grid-template-columns: minmax(150px, 25%) 1fr;
  border-bottom: 1px solid var(--clr-outline);
  min-height: var(--sp-xl);
  align-items: center;
}

.mp-gantt-row:last-child {
  border-bottom: none;
}

.mp-gantt-header {
  font-weight: 700;
  color: var(--clr-on-surface-variant);
  font-size: var(--ts-label);
  text-transform: uppercase;
  background-color: var(--clr-surface);
  position: sticky;
  top: 0;
  z-index: 2;
  border-bottom: 2px solid var(--clr-outline);
}

.mp-gantt-label-col {
  padding: var(--sp-s) var(--sp-m);
  font-size: var(--ts-body);
  color: var(--clr-on-surface);
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
  background-color: inherit; /* For sticky if needed */
  z-index: 3;
}

.mp-gantt-track-col {
  position: relative;
  height: 100%;
  width: 100%;
  min-width: 300px; /* Ensure some width for timeline */
}

/* Ticks in Header */
.mp-gantt-tick {
  position: absolute;
  transform: translateX(-50%); /* Center on the date */
  white-space: nowrap;
  font-size: var(--ts-label);
  bottom: var(--sp-xs);
}

/* Grid Lines */
.mp-gantt-grid-lines {
  position: absolute;
  inset: 0;
  pointer-events: none;
}

.mp-gantt-grid-line {
  position: absolute;
  top: 0;
  bottom: 0;
  width: 1px;
  background-color: var(--clr-outline);
  opacity: 0.3;
}

/* Bars */
.mp-gantt-bar-container {
  position: absolute;
  top: 50%;
  transform: translateY(-50%);
  height: var(--sp-l);
  border-radius: var(--sp-s);
  /* Default background if slot doesn't override */
  background-color: var(--clr-primary);
  min-width: 4px; /* Visibility for short tasks */
  transition: all 0.3s ease;
}

.mp-gantt-bar {
  width: 100%;
  height: 100%;
  /* Internal styling if needed */
}

/* Variants */
.mp-gantt--dense .mp-gantt-row {
  min-height: var(--sp-l);
}

.mp-gantt--dense .mp-gantt-bar-container {
  height: var(--sp-m);
}

.mp-gantt-empty {
  padding: var(--sp-xl);
  text-align: center;
  color: var(--clr-on-surface-variant);
  font-style: italic;
}

/* Responsive */
@container (max-width: 500px) {
  .mp-gantt-row {
    grid-template-columns: 100px 1fr;
  }
  .mp-gantt-tick {
    font-size: 0.7em;
  }
}
</style>
