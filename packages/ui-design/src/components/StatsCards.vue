<template>
  <div class="nx-stats-grid">
    <div 
      v-for="stat in stats" 
      :key="stat.title" 
      class="nx-stat-card"
    >
      <div class="nx-stat-header">
        <div class="nx-stat-icon-wrapper" :class="[stat.bg, stat.color]">
          <el-icon class="nx-stat-icon">
            <component :is="stat.icon" />
          </el-icon>
        </div>
        <div v-if="stat.trend" class="nx-stat-trend" :class="stat.trendUp ? 'up' : 'down'">
          {{ stat.trend }}
          <el-icon v-if="stat.trendIcon" class="nx-stat-trend-icon">
            <component :is="stat.trendIcon" />
          </el-icon>
        </div>
      </div>
      <div class="nx-stat-content">
        <h3 class="nx-stat-title">{{ stat.title }}</h3>
        <div class="nx-stat-value">{{ stat.value }}</div>
      </div>
    </div>
  </div>
</template>

<script setup lang="ts">
import type { Component } from "vue";

export interface StatItem {
    title: string;
    value: string | number;
    icon: Component | string;
    color?: string;
    bg?: string;
    trend?: string;
    trendUp?: boolean;
    trendIcon?: Component | string;
}

defineProps<{
    stats: StatItem[];
}>();
</script>

<style lang="scss" scoped>
.nx-stats-grid {
  display: grid;
  grid-template-columns: repeat(1, minmax(0, 1fr));
  gap: 1.5rem;

  @media (min-width: 768px) {
    grid-template-columns: repeat(2, minmax(0, 1fr));
  }

  @media (min-width: 1024px) {
    grid-template-columns: repeat(4, minmax(0, 1fr));
  }
}

.nx-stat-card {
  background-color: #ffffff;
  padding: 1.5rem;
  border-radius: 1rem;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  border: 1px solid #f1f5f9;
  transition: transform 0.2s;

  &:hover {
    transform: translateY(-0.25rem);
  }

  :deep(.dark) & {
    background-color: #1e293b;
    border-color: #334155;
  }
}

.nx-stat-header {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  margin-bottom: 1rem;
}

.nx-stat-icon-wrapper {
  padding: 0.75rem;
  border-radius: 0.75rem;
  display: flex;
  align-items: center;
  justify-content: center;

  .nx-stat-icon {
    font-size: 1.25rem;
  }
}

.nx-stat-trend {
  display: flex;
  align-items: center;
  font-size: 0.75rem;
  font-weight: 500;
  padding: 0.25rem 0.5rem;
  border-radius: 9999px;

  &.up {
    color: #059669;
    background-color: #ecfdf5;
    
    :deep(.dark) & {
      background-color: rgba(6, 78, 59, 0.3);
    }
  }

  &.down {
    color: #dc2626;
    background-color: #fef2f2;
  }

  .nx-stat-trend-icon {
    margin-left: 0.25rem;
  }
}

.nx-stat-content {
  .nx-stat-title {
    color: #64748b;
    font-size: 0.875rem;
    font-weight: 500;
    margin-bottom: 0.25rem;
    margin-top: 0;
  }

  .nx-stat-value {
    font-size: 1.5rem;
    font-weight: 700;
    color: #1e293b;

    :deep(.dark) & {
      color: #ffffff;
    }
  }
}
</style>
