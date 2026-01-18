<template>
  <div class="nx-insight-container">
    <div class="nx-insight-header">
      <div :class="['nx-header-icon-box', headerBgClass]">
        <el-icon v-if="headerIcon">
          <component :is="headerIcon" />
        </el-icon>
      </div>
      <div class="nx-header-title">{{ title }}</div>
    </div>

    <div class="nx-insight-list">
      <div 
        v-for="item in items" 
        :key="item.id"
        class="nx-insight-item"
        :class="item.borderClass"
        @click="$emit('item-click', item)"
      >
        <div v-if="item.icon" class="nx-item-icon-wrapper">
           <el-icon :class="item.iconClass">
             <component :is="item.icon" />
           </el-icon>
        </div>
        <div class="nx-item-body">
           <p class="nx-item-content">{{ item.content }}</p>
           <button 
             v-if="item.action" 
             class="nx-item-action"
             @click.stop="$emit('action-click', item)"
           >
             {{ item.action }} <span class="nx-action-arrow">→</span>
           </button>
        </div>
      </div>
    </div>

    <!-- Decorative Background -->
    <div class="nx-decoration-top"></div>
    <div class="nx-decoration-bottom"></div>
  </div>
</template>

<script setup lang="ts">
import type { Component } from "vue";

export interface InsightItem {
    id: string;
    content: string;
    icon?: Component | string;
    iconClass?: string;
    borderClass?: string;
    action?: string;
    raw?: any;
}

defineProps<{
    title: string;
    headerIcon?: Component | string;
    headerBgClass?: string;
    items: InsightItem[];
}>();

defineEmits<{
    (e: "item-click", item: InsightItem): void;
    (e: "action-click", item: InsightItem): void;
}>();
</script>

<style lang="scss" scoped>
.nx-insight-container {
  background: linear-gradient(to bottom right, #f5f3ff, #f3e8ff);
  border-radius: 1rem;
  border: 1px solid #e0e7ff;
  padding: 1.5rem;
  position: relative;
  overflow: hidden;
  height: 100%;
}

.nx-insight-header {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  margin-bottom: 1rem;
  position: relative;
  z-index: 10;

  .nx-header-icon-box {
    width: 2rem;
    height: 2rem;
    border-radius: 0.5rem;
    background-color: #4f46e5;
    color: #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
    box-shadow: 0 4px 6px -1px rgba(199, 210, 254, 1);
  }

  .nx-header-title {
    font-weight: 700;
    color: #312e81;
    font-size: 1.125rem;
  }
}

.nx-insight-list {
  display: flex;
  flex-direction: column;
  gap: 0.75rem;
  position: relative;
  z-index: 10;
}

.nx-insight-item {
  background-color: rgba(255, 255, 255, 0.8);
  backdrop-filter: blur(4px);
  padding: 0.75rem;
  border-radius: 0.75rem;
  border: 1px solid rgba(255, 255, 255, 0.5);
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  transition: all 0.2s;
  cursor: pointer;
  display: flex;
  gap: 0.75rem;

  &:hover {
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
  }

  .nx-item-icon-wrapper {
    margin-top: 0.125rem;
  }

  .nx-item-body {
    flex: 1;

    .nx-item-content {
      font-size: 0.875rem;
      color: #374151;
      line-height: 1.25;
      margin: 0;
    }

    .nx-item-action {
      margin-top: 0.5rem;
      font-size: 0.75rem;
      font-weight: 600;
      color: #4f46e5;
      background: none;
      border: none;
      padding: 0;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 0.25rem;

      &:hover {
        text-decoration: underline;
        color: #4338ca;
      }

      .nx-action-arrow {
        font-size: 0.625rem;
      }
    }
  }
}

.nx-decoration-top {
  position: absolute;
  top: -1.5rem;
  right: -1.5rem;
  width: 8rem;
  height: 8rem;
  background-color: rgba(233, 213, 255, 0.3);
  border-radius: 9999px;
  filter: blur(24px);
  pointer-events: none;
}

.nx-decoration-bottom {
  position: absolute;
  bottom: -1.5rem;
  left: -1.5rem;
  width: 8rem;
  height: 8rem;
  background-color: rgba(224, 231, 255, 0.3);
  border-radius: 9999px;
  filter: blur(24px);
  pointer-events: none;
}
</style>
