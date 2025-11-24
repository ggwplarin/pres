<script setup lang="ts" generic="T extends Record<string, any>">
import { computed } from "vue";
import { resolveSpacing } from "../utils/spacing";
import type { SpacingName } from "../types/tokens";

export interface TableColumn {
  key: string;
  label?: string;
  align?: "start" | "center" | "end" | "justify";
  width?: string;
}

const props = defineProps<{
  items: T[];
  columns: TableColumn[];
  variant?: "default" | "outlined";
  dense?: boolean;
  hover?: boolean;
  striped?: boolean;
  padding?: SpacingName | string | number;
  headerTextSize?: "label" | "body" | "title" | "headline" | "display";
  bodyTextSize?: "label" | "body" | "title" | "headline" | "display";
}>();

const emit = defineEmits<{
  (e: "row-click", item: T, index: number): void;
}>();

const tableClasses = computed(() => ({
  "mp-table--outlined": props.variant === "outlined",
  "mp-table--dense": props.dense && !props.padding, // Dense overridden if explicit padding provided
  "mp-table--hover": props.hover,
  "mp-table--striped": props.striped,
}));

const customPadding = computed(() => {
  if (props.padding) return resolveSpacing(props.padding);
  return null;
});

const headerFontSize = computed(() => {
  const size = props.headerTextSize ?? "label";
  return `var(--ts-${size})`;
});

const bodyFontSize = computed(() => {
  const size = props.bodyTextSize ?? "body";
  return `var(--ts-${size})`;
});

const thStyle = (col: TableColumn) => ({
  textAlign: col.align ?? "start",
  width: col.width,
  ...(customPadding.value ? { padding: customPadding.value } : {}),
});

const tdStyle = (col: TableColumn) => ({
  textAlign: col.align ?? "start",
  ...(customPadding.value ? { padding: customPadding.value } : {}),
});
</script>

<template>
  <div class="mp-table-container" :class="tableClasses">
    <table class="mp-table" :style="{ '--table-body-font-size': bodyFontSize }">
      <thead :style="{ '--table-header-font-size': headerFontSize }">
        <tr>
          <th v-for="col in props.columns" :key="col.key" :style="thStyle(col)">
            <slot :name="`header-${col.key}`" :column="col">
              {{ col.label ?? col.key }}
            </slot>
          </th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, index) in props.items"
          :key="index"
          @click="emit('row-click', item, index)"
        >
          <td v-for="col in props.columns" :key="col.key" :style="tdStyle(col)">
            <slot
              :name="`cell-${col.key}`"
              :item="item"
              :index="index"
              :value="item[col.key]"
            >
              {{ item[col.key] }}
            </slot>
          </td>
        </tr>
        <tr v-if="props.items.length === 0">
          <td :colspan="props.columns.length" class="mp-table-empty">
            <slot name="empty">Нет данных</slot>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<style scoped>
.mp-table-container {
  width: 100%;
  overflow-x: auto;
  container-type: inline-size;
  /* Base border radius for container */
  border-radius: var(--sp-m);
  background-color: var(--clr-surface);
}

.mp-table {
  width: 100%;
  border-collapse: separate; /* Allows border-radius on container to work better with borders */
  border-spacing: 0;
  font-family: "Roboto", sans-serif;
  font-size: var(--table-body-font-size);
  color: var(--clr-on-surface);
}

th,
td {
  padding: var(--sp-m);
  border-bottom: 1px solid var(--clr-outline);
  transition: background-color 0.2s ease;
}

/* Header styles */
th {
  background-color: var(--clr-surface);
  font-weight: 700;
  color: var(--clr-on-surface-variant);
  font-size: var(--table-header-font-size);
  text-transform: uppercase;
  letter-spacing: 0.05em;
  position: sticky;
  top: 0;
  z-index: 1;
}

/* Last row border removal if inside outlined container, or just generally nice */
tr:last-child td {
  border-bottom: none;
}

/* Empty state */
.mp-table-empty {
  text-align: center;
  padding: var(--sp-xl);
  color: var(--clr-on-surface-variant);
  font-style: italic;
}

/* Variants */

/* Outlined: Adds a border around the container */
.mp-table-container.mp-table--outlined {
  border: 1px solid var(--clr-outline);
}

/* Dense: Reduces padding */
.mp-table-container.mp-table--dense th,
.mp-table-container.mp-table--dense td {
  padding: var(--sp-s);
  font-size: calc(var(--ts-body) * 0.9);
}

/* Striped: Alternating row colors */
.mp-table-container.mp-table--striped tbody tr:nth-child(even) td {
  background-color: color-mix(in srgb, var(--clr-on-surface) 3%, transparent);
}

/* Responsive typography or spacing adjustments could go here */
@container (max-width: 500px) {
  th,
  td {
    padding: var(--sp-s);
  }
}
</style>
