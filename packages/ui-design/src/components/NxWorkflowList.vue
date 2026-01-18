<script setup lang="ts">
import { Clock, Plus, ArrowRight } from "@element-plus/icons-vue";

export interface WorkflowItem {
    id: string;
    title: string;
    type: string;
    initiator: string;
    startTime: string;
    status: "pending" | "approved" | "rejected";
    step: string;
}

export interface WorkflowStat {
    label: string;
    value: number | string;
    color: "orange" | "blue" | "green" | "red";
    icon: any;
}

interface Props {
    title?: string;
    description?: string;
    workflows: WorkflowItem[];
    stats?: WorkflowStat[];
}

withDefaults(defineProps<Props>(), {
    title: "Workflow",
    description: "Manage approvals and processes.",
    stats: () => [],
});

defineEmits<{
    (e: "create"): void;
    (e: "select", workflow: WorkflowItem): void;
}>();
</script>

<template>
  <div class="nx-workflow-list">
    <div class="nx-workflow-list__header">
       <div class="nx-workflow-list__info">
          <h1 class="nx-workflow-list__title">{{ title }}</h1>
          <p class="nx-workflow-list__description">{{ description }}</p>
       </div>
       <el-button type="primary" size="large" round @click="$emit('create')">
          <template #icon><el-icon><Plus /></el-icon></template>
          New Request
       </el-button>
    </div>

    <!-- Quick Stats -->
    <div v-if="stats.length" class="nx-workflow-stats">
       <div 
         v-for="(stat, i) in stats" 
         :key="i" 
         class="nx-workflow-stat-card"
         :class="[`nx-workflow-stat-card--${stat.color}`]"
       >
          <div class="nx-workflow-stat-card__content">
             <div class="nx-workflow-stat-card__value">{{ stat.value }}</div>
             <div class="nx-workflow-stat-card__label">{{ stat.label }}</div>
          </div>
          <el-icon class="nx-workflow-stat-card__icon">
            <component :is="stat.icon" />
          </el-icon>
       </div>
    </div>

    <!-- Workflow Table -->
    <div class="nx-workflow-container">
       <div class="nx-workflow-table-header">
          <div class="nx-workflow-col nx-workflow-col--title">Request</div>
          <div class="nx-workflow-col nx-workflow-col--initiator">Initiator</div>
          <div class="nx-workflow-col nx-workflow-col--status">Status</div>
          <div class="nx-workflow-col nx-workflow-col--step">Current Step</div>
          <div class="nx-workflow-col nx-workflow-col--action"></div>
       </div>

       <div class="nx-workflow-items">
          <div 
            v-for="item in workflows" 
            :key="item.id" 
            class="nx-workflow-item"
            @click="$emit('select', item)"
          >
             <div class="nx-workflow-col nx-workflow-col--title">
                <div class="nx-workflow-item__title">{{ item.title }}</div>
                <div class="nx-workflow-item__meta">{{ item.type }} • {{ item.startTime }}</div>
             </div>
             <div class="nx-workflow-col nx-workflow-col--initiator">
                <div class="nx-workflow-item__initiator">
                   <div class="nx-workflow-item__avatar">{{ item.initiator[0] }}</div>
                   <span>{{ item.initiator }}</span>
                </div>
             </div>
             <div class="nx-workflow-col nx-workflow-col--status">
                <span class="nx-workflow-status-tag" :class="[`nx-workflow-status-tag--${item.status}`]">
                   {{ item.status }}
                </span>
             </div>
             <div class="nx-workflow-col nx-workflow-col--step">
                <span class="nx-workflow-item__step">{{ item.step }}</span>
             </div>
             <div class="nx-workflow-col nx-workflow-col--action">
                <el-icon class="nx-workflow-item__arrow"><ArrowRight /></el-icon>
             </div>
          </div>
       </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.nx-workflow-list {
  display: flex;
  flex-direction: column;
  height: 100%;

  &__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 2rem;
  }

  &__title {
    font-size: 1.875rem;
    font-weight: 700;
    color: #111827;
    margin-bottom: 0.5rem;
  }

  &__description {
    color: #6b7280;
  }
}

.nx-workflow-stats {
  display: grid;
  grid-template-columns: repeat(1, minmax(0, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;

  @media (min-width: 640px) {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  @media (min-width: 1024px) {
    grid-template-columns: repeat(4, minmax(0, 1fr));
  }
}

.nx-workflow-stat-card {
  padding: 1rem;
  border-radius: 0.75rem;
  border: 1px solid transparent;
  display: flex;
  align-items: center;
  justify-content: space-between;

  &--orange {
    background-color: #fffaf0;
    border-color: #ffedd5;
    .nx-workflow-stat-card__value { color: #ea580c; }
    .nx-workflow-stat-card__label { color: #f97316; }
    .nx-workflow-stat-card__icon { color: #fdba74; }
  }

  &--blue {
    background-color: #eff6ff;
    border-color: #dbeafe;
    .nx-workflow-stat-card__value { color: #2563eb; }
    .nx-workflow-stat-card__label { color: #3b82f6; }
    .nx-workflow-stat-card__icon { color: #93c5fd; }
  }

  &--green {
    background-color: #f0fdf4;
    border-color: #dcfce7;
    .nx-workflow-stat-card__value { color: #16a34a; }
    .nx-workflow-stat-card__label { color: #22c55e; }
    .nx-workflow-stat-card__icon { color: #86efac; }
  }

  &--red {
    background-color: #fef2f2;
    border-color: #fee2e2;
    .nx-workflow-stat-card__value { color: #dc2626; }
    .nx-workflow-stat-card__label { color: #ef4444; }
    .nx-workflow-stat-card__icon { color: #fca5a5; }
  }

  &__value {
    font-size: 1.5rem;
    font-weight: 700;
  }

  &__label {
    font-size: 0.75rem;
    font-weight: 600;
    text-transform: uppercase;
    margin-top: 0.25rem;
  }

  &__icon {
    font-size: 1.875rem;
  }
}

.nx-workflow-container {
  background-color: #ffffff;
  border-radius: 1rem;
  border: 1px solid #f3f4f6;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.nx-workflow-table-header {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  padding: 0.75rem 1.5rem;
  background-color: rgba(249, 250, 251, 0.5);
  border-bottom: 1px solid #f3f4f6;
  font-size: 0.75rem;
  font-weight: 600;
  color: #6b7280;
  text-transform: uppercase;
  letter-spacing: 0.05em;
}

.nx-workflow-col {
  &--title { grid-column: span 4; }
  &--initiator { grid-column: span 3; }
  &--status { grid-column: span 2; }
  &--step { grid-column: span 2; }
  &--action { grid-column: span 1; text-align: right; }
}

.nx-workflow-items {
  display: flex;
  flex-direction: column;

  & > * + * {
    border-top: 1px solid #f9fafb;
  }
}

.nx-workflow-item {
  display: grid;
  grid-template-columns: repeat(12, minmax(0, 1fr));
  padding: 1.25rem 1.5rem;
  align-items: center;
  cursor: pointer;
  transition: background-color 0.2s;

  &:hover {
    background-color: #f9fafb;

    .nx-workflow-item__arrow {
      transform: translateX(4px);
      color: #2563eb;
    }
  }

  &__title {
    font-weight: 600;
    color: #111827;
    margin-bottom: 0.25rem;
  }

  &__meta {
    font-size: 0.75rem;
    color: #9ca3af;
  }

  &__initiator {
    display: flex;
    align-items: center;
    gap: 0.75rem;
    font-size: 0.875rem;
    color: #4b5563;
  }

  &__avatar {
    width: 1.5rem;
    height: 1.5rem;
    border-radius: 9999px;
    background-color: #f3f4f6;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.75rem;
    font-weight: 600;
    color: #6b7280;
  }

  &__step {
    font-size: 0.875rem;
    color: #4b5563;
  }

  &__arrow {
    color: #d1d5db;
    transition: all 0.2s;
  }
}

.nx-workflow-status-tag {
  display: inline-block;
  padding: 0.25rem 0.625rem;
  border-radius: 9999px;
  font-size: 0.75rem;
  font-weight: 500;
  text-transform: capitalize;

  &--pending { background-color: #fef3c7; color: #92400e; }
  &--approved { background-color: #dcfce7; color: #166534; }
  &--rejected { background-color: #fee2e2; color: #991b1b; }
}
</style>
