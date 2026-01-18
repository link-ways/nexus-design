<template>
  <div class="nx-system-status">
     <!-- Decorative background -->
     <div class="nx-decoration-top"></div>
     <div class="nx-decoration-bottom"></div>

     <h3 class="nx-status-title">{{ title }}</h3>
     <div class="nx-status-items">
        <div v-for="item in items" :key="item.label" class="nx-status-item">
           <div class="nx-item-header">
             <span class="nx-item-label">{{ item.label }}</span>
             <span :class="['nx-item-value', item.valueClass]">{{ item.valueText }}</span>
           </div>
           <div class="nx-progress-bg">
             <div :class="['nx-progress-bar', item.barClass]" :style="{ width: item.percentage + '%' }"></div>
           </div>
        </div>
     </div>

     <div v-if="actionText" class="nx-status-footer">
        <button 
          class="nx-action-button"
          @click="$emit('action')"
        >
          {{ actionText }}
        </button>
     </div>
  </div>
</template>

<script setup lang="ts">
export interface StatusItem {
    label: string;
    valueText: string;
    percentage: number;
    valueClass?: string;
    barClass?: string;
}

defineProps<{
    title: string;
    items: StatusItem[];
    actionText?: string;
}>();

defineEmits<{
    (e: "action"): void;
}>();
</script>

<style lang="scss" scoped>
.nx-system-status {
  background: linear-gradient(to bottom right, #1e293b, #0f172a);
  border-radius: 1rem;
  box-shadow: 0 10px 15px -3px rgba(0, 0, 0, 0.1);
  padding: 1.5rem;
  color: #ffffff;
  position: relative;
  overflow: hidden;
}

.nx-decoration-top {
  position: absolute;
  top: 0;
  right: 0;
  width: 8rem;
  height: 8rem;
  background-color: rgba(59, 130, 246, 0.1);
  border-radius: 9999px;
  filter: blur(24px);
  margin-right: -2.5rem;
  margin-top: -2.5rem;
}

.nx-decoration-bottom {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 6rem;
  height: 6rem;
  background-color: rgba(168, 85, 247, 0.1);
  border-radius: 9999px;
  filter: blur(24px);
  margin-left: -2.5rem;
  margin-bottom: -2.5rem;
}

.nx-status-title {
  font-size: 1.125rem;
  font-weight: 700;
  margin-bottom: 1rem;
  margin-top: 0;
  position: relative;
  z-index: 10;
}

.nx-status-items {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  position: relative;
  z-index: 10;
}

.nx-status-item {
  .nx-item-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    font-size: 0.875rem;
    margin-bottom: 0.5rem;

    .nx-item-label {
      color: #94a3b8;
    }

    .nx-item-value {
      font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
      color: #34d399; // Default emerald-400
    }
  }

  .nx-progress-bg {
    width: 100%;
    background-color: rgba(51, 65, 85, 0.5);
    border-radius: 9999px;
    height: 0.5rem;

    .nx-progress-bar {
      height: 100%;
      border-radius: 9999px;
      background-color: #10b981; // Default emerald-500
      transition: width 0.3s ease;
    }
  }
}

.nx-status-footer {
  margin-top: 2rem;
  padding-top: 1.5rem;
  border-top: 1px solid rgba(51, 65, 85, 0.5);
  position: relative;
  z-index: 10;

  .nx-action-button {
    width: 100%;
    padding: 0.5rem 0;
    background-color: rgba(255, 255, 255, 0.1);
    border: none;
    border-radius: 0.5rem;
    color: #ffffff;
    font-size: 0.875rem;
    font-weight: 500;
    cursor: pointer;
    transition: background-color 0.2s;

    &:hover {
      background-color: rgba(255, 255, 255, 0.2);
    }
  }
}
</style>
