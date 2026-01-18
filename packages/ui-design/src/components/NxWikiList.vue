<script setup lang="ts">
import { Document, Folder, More, Star, Plus, Files, VideoPlay, Headset, Picture, Box } from "@element-plus/icons-vue";
import { computed } from "vue";

export type WikiType = "doc" | "sheet" | "slide" | "pdf" | "video" | "audio" | "image" | "archive" | "other";

export interface WikiItem {
    id: string;
    title: string;
    updatedAt: string;
    author: string;
    type: WikiType;
}

interface Props {
    title?: string;
    description?: string;
    items: WikiItem[];
    recentItems?: WikiItem[];
}

const props = withDefaults(defineProps<Props>(), {
    title: "Wiki & Knowledge",
    description: "Access and manage all your documents, media, and knowledge.",
    recentItems: () => [],
});

defineEmits<{
    (e: "create"): void;
    (e: "select", item: WikiItem): void;
    (e: "star", item: WikiItem): void;
    (e: "more", item: WikiItem): void;
}>();

const getIcon = (type: WikiType) => {
    switch (type) {
        case "video":
            return VideoPlay;
        case "audio":
            return Headset;
        case "image":
            return Picture;
        case "archive":
            return Box;
        case "pdf":
            return Files;
        default:
            return Document;
    }
};
</script>

<template>
  <div class="nx-wiki-list">
    <div class="nx-wiki-list__header">
       <div class="nx-wiki-list__info">
          <h1 class="nx-wiki-list__title">{{ title }}</h1>
          <p class="nx-wiki-list__description">{{ description }}</p>
       </div>
       <el-button type="primary" size="large" round @click="$emit('create')">
          <template #icon><el-icon><Plus /></el-icon></template>
          New Content
       </el-button>
    </div>

    <!-- Quick Access / Recent -->
    <div v-if="recentItems.length" class="nx-wiki-list__quick-access">
       <div 
         v-for="(item, i) in recentItems" 
         :key="i" 
         class="nx-wiki-card"
         @click="$emit('select', item)"
       >
          <div class="nx-wiki-card__icon" :class="[`nx-wiki-card__icon--${item.type}`]">
             <el-icon :size="20"><component :is="getIcon(item.type)" /></el-icon>
          </div>
          <div class="nx-wiki-card__title">{{ item.title }}</div>
          <div class="nx-wiki-card__meta">{{ item.updatedAt }}</div>
       </div>
    </div>

    <!-- Wiki List -->
    <div class="nx-wiki-list__container">
       <div class="nx-wiki-list__container-header">
          <div class="nx-wiki-list__container-title">Recent Files</div>
          <el-icon class="nx-wiki-list__folder-icon"><Folder /></el-icon>
       </div>
       <div class="nx-wiki-list__items">
          <div 
            v-for="item in items" 
            :key="item.id" 
            class="nx-wiki-item"
            @click="$emit('select', item)"
          >
             <div class="nx-wiki-item__content">
                <div class="nx-wiki-item__icon-wrapper" :class="[`nx-wiki-item__icon-wrapper--${item.type}`]">
                   <el-icon><component :is="getIcon(item.type)" /></el-icon>
                </div>
                <div class="nx-wiki-item__info">
                   <div class="nx-wiki-item__title">{{ item.title }}</div>
                   <div class="nx-wiki-item__meta">Edited {{ item.updatedAt }} by {{ item.author }}</div>
                </div>
             </div>
             <div class="nx-wiki-item__actions">
                <el-button circle size="small" @click.stop="$emit('star', item)">
                  <el-icon><Star /></el-icon>
                </el-button>
                <el-button circle size="small" @click.stop="$emit('more', item)">
                  <el-icon><More /></el-icon>
                </el-button>
             </div>
          </div>
       </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.nx-wiki-list {
  max-width: 64rem;
  margin: 0 auto;

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

    :deep(.dark) & {
      color: #f9fafb;
    }
  }

  &__description {
    color: #6b7280;

    :deep(.dark) & {
      color: #9ca3af;
    }
  }

  &__quick-access {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 1.5rem;
    margin-bottom: 2.5rem;

    @media (min-width: 768px) {
      grid-template-columns: repeat(4, minmax(0, 1fr));
    }
  }

  &__container {
    background-color: #ffffff;
    border-radius: 1rem;
    border: 1px solid #f3f4f6;
    box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
    overflow: hidden;

    :deep(.dark) & {
      background-color: #1e293b;
      border-color: #334155;
    }

    &-header {
      padding: 1rem 1.5rem;
      border-bottom: 1px solid #f9fafb;
      display: flex;
      align-items: center;
      justify-content: space-between;
      background-color: rgba(249, 250, 251, 0.5);

      :deep(.dark) & {
        background-color: rgba(15, 23, 42, 0.5);
        border-bottom-color: #334155;
      }
    }

    &-title {
      font-weight: 700;
      color: #374151;

      :deep(.dark) & {
        color: #e2e8f0;
      }
    }
  }

  &__folder-icon {
    color: #9ca3af;
    cursor: pointer;
  }

  &__items {
    display: flex;
    flex-direction: column;

    & > * + * {
      border-top: 1px solid #f9fafb;

      :deep(.dark) & {
        border-top-color: #334155;
      }
    }
  }
}

.nx-wiki-card {
  background-color: #ffffff;
  padding: 1rem;
  border-radius: 0.75rem;
  border: 1px solid #f3f4f6;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  cursor: pointer;
  transition: all 0.2s;

  :deep(.dark) & {
    background-color: #1e293b;
    border-color: #334155;
  }

  &:hover {
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
    transform: translateY(-2px);
  }

  &__icon {
    width: 2.5rem;
    height: 2.5rem;
    border-radius: 0.5rem;
    background-color: #eef2ff;
    color: #4f46e5;
    display: flex;
    align-items: center;
    justify-content: center;
    margin-bottom: 0.75rem;

    &--sheet { background-color: #f0fdf4; color: #16a34a; }
    &--slide { background-color: #fff7ed; color: #ea580c; }
    &--pdf { background-color: #fef2f2; color: #dc2626; }
    &--video { background-color: #faf5ff; color: #9333ea; }
    &--audio { background-color: #ecfeff; color: #0891b2; }
    &--image { background-color: #f0f9ff; color: #0284c7; }
    &--archive { background-color: #f8fafc; color: #475569; }

    :deep(.dark) & {
      opacity: 0.8;
    }
  }

  &__title {
    font-weight: 500;
    color: #111827;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    margin-bottom: 0.25rem;

    :deep(.dark) & {
      color: #f9fafb;
    }
  }

  &__meta {
    font-size: 0.75rem;
    color: #9ca3af;
  }
}

.nx-wiki-item {
  padding: 1rem 1.5rem;
  display: flex;
  align-items: center;
  justify-content: space-between;
  transition: background-color 0.2s;
  cursor: pointer;

  &:hover {
    background-color: #f9fafb;

    :deep(.dark) & {
      background-color: #334155;
    }

    .nx-wiki-item__title {
      color: #2563eb;
    }

    .nx-wiki-item__actions {
      opacity: 1;
    }
  }

  &__content {
    display: flex;
    align-items: center;
    gap: 1rem;
  }

  &__icon-wrapper {
    width: 2rem;
    height: 2rem;
    border-radius: 0.25rem;
    background-color: #f3f4f6;
    display: flex;
    align-items: center;
    justify-content: center;
    color: #6b7280;

    :deep(.dark) & {
      background-color: #475569;
      color: #cbd5e1;
    }

    &--sheet { color: #16a34a; }
    &--slide { color: #ea580c; }
    &--pdf { color: #dc2626; }
    &--video { color: #9333ea; }
    &--audio { color: #0891b2; }
    &--image { color: #0284c7; }
    &--archive { color: #475569; }
  }

  &__info {
    display: flex;
    flex-direction: column;
  }

  &__title {
    font-weight: 500;
    color: #374151;
    transition: color 0.2s;

    :deep(.dark) & {
      color: #e2e8f0;
    }
  }

  &__meta {
    font-size: 0.75rem;
    color: #9ca3af;
  }

  &__actions {
    display: flex;
    gap: 0.5rem;
    opacity: 0;
    transition: opacity 0.2s;

    @media (max-width: 640px) {
      opacity: 1;
    }
  }
}
</style>
