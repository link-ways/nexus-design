<script setup lang="ts">
import { VideoCamera, Calendar, Clock, Plus, More } from "@element-plus/icons-vue";

export interface MeetingItem {
    id: string;
    title: string;
    time: string;
    duration: string;
    participants: string[];
    status: "upcoming" | "ongoing" | "finished";
}

interface Props {
    title?: string;
    description?: string;
    meetings: MeetingItem[];
}

withDefaults(defineProps<Props>(), {
    title: "Meetings",
    description: "Manage your schedule and join video calls.",
});

defineEmits<{
    (e: "create"): void;
    (e: "join", id: string): void;
    (e: "more", meeting: MeetingItem): void;
}>();
</script>

<template>
  <div class="nx-meeting-list">
    <div v-if="title || description" class="nx-meeting-list__header">
       <div class="nx-meeting-list__info">
          <h1 v-if="title" class="nx-meeting-list__title">{{ title }}</h1>
          <p v-if="description" class="nx-meeting-list__description">{{ description }}</p>
       </div>
       <el-button type="primary" size="large" round @click="$emit('create')">
          <template #icon><el-icon><Plus /></el-icon></template>
          New Meeting
       </el-button>
    </div>

    <!-- Upcoming Meetings -->
    <div class="nx-meeting-list__items">
       <div v-if="!title && !description" class="flex justify-end mb-4">
         <el-button type="primary" size="large" round @click="$emit('create')">
            <template #icon><el-icon><Plus /></el-icon></template>
            New Meeting
         </el-button>
       </div>
       <div 
         v-for="meeting in meetings" 
         :key="meeting.id" 
         class="nx-meeting-item"
         :class="[`nx-meeting-item--${meeting.status}`]"
       >
          <div class="nx-meeting-item__status-indicator"></div>
          
          <div class="nx-meeting-item__content">
             <div class="nx-meeting-item__main">
                <div class="nx-meeting-item__time-box">
                   <div class="nx-meeting-item__time">{{ meeting.time }}</div>
                   <div class="nx-meeting-item__duration">{{ meeting.duration }}</div>
                </div>
                
                <div class="nx-meeting-item__info">
                   <div class="nx-meeting-item__title-row">
                      <h3 class="nx-meeting-item__title">{{ meeting.title }}</h3>
                      <span v-if="meeting.status === 'ongoing'" class="nx-meeting-item__badge nx-meeting-item__badge--live">LIVE</span>
                   </div>
                   <div class="nx-meeting-item__meta">
                      <div class="nx-meeting-item__participants">
                         <div v-for="(p, i) in meeting.participants.slice(0, 3)" :key="i" class="nx-meeting-item__avatar">
                            {{ p[0] }}
                         </div>
                         <div v-if="meeting.participants.length > 3" class="nx-meeting-item__avatar nx-meeting-item__avatar--more">
                            +{{ meeting.participants.length - 3 }}
                         </div>
                         <span class="nx-meeting-item__participant-count">{{ meeting.participants.length }} participants</span>
                      </div>
                   </div>
                </div>
             </div>

             <div class="nx-meeting-item__actions">
                <el-button 
                  :type="meeting.status === 'ongoing' ? 'danger' : 'default'" 
                  round 
                  @click="$emit('join', meeting.id)"
                >
                   {{ meeting.status === 'ongoing' ? 'Join Now' : 'Details' }}
                </el-button>
                <el-button circle size="small" @click="$emit('more', meeting)">
                   <el-icon><More /></el-icon>
                </el-button>
             </div>
          </div>
       </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.nx-meeting-list {
  display: flex;
  flex-direction: column;
  height: 100%;

  &__header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    margin-bottom: 2rem;
    flex-shrink: 0;
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

  &__items {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    overflow-y: auto;
    flex: 1;
    padding-bottom: 1rem;
  }
}

.nx-meeting-item {
  background-color: #ffffff;
  border-radius: 1rem;
  border: 1px solid #f3f4f6;
  box-shadow: 0 1px 2px 0 rgba(0, 0, 0, 0.05);
  display: flex;
  overflow: hidden;
  transition: all 0.2s;

  &:hover {
    box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1);
    border-color: #e5e7eb;
  }

  &--ongoing {
    border-left: 4px solid #ef4444;
    .nx-meeting-item__status-indicator { background-color: #ef4444; }
  }

  &--upcoming {
    border-left: 4px solid #3b82f6;
    .nx-meeting-item__status-indicator { background-color: #3b82f6; }
  }

  &--finished {
    border-left: 4px solid #9ca3af;
    opacity: 0.7;
    .nx-meeting-item__status-indicator { background-color: #9ca3af; }
  }

  &__status-indicator {
    width: 0.25rem;
  }

  &__content {
    flex: 1;
    padding: 1.25rem 1.5rem;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  &__main {
    display: flex;
    align-items: center;
    gap: 2rem;
  }

  &__time-box {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-width: 5rem;
  }

  &__time {
    font-size: 1.125rem;
    font-weight: 700;
    color: #111827;
  }

  &__duration {
    font-size: 0.75rem;
    color: #6b7280;
    font-weight: 500;
  }

  &__info {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
  }

  &__title-row {
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }

  &__title {
    font-size: 1.125rem;
    font-weight: 600;
    color: #111827;
    margin: 0;
  }

  &__badge {
    font-size: 0.625rem;
    font-weight: 700;
    padding: 0.125rem 0.375rem;
    border-radius: 0.25rem;

    &--live {
      background-color: #fee2e2;
      color: #ef4444;
      animation: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
    }
  }

  &__meta {
    display: flex;
    align-items: center;
  }

  &__participants {
    display: flex;
    align-items: center;
  }

  &__avatar {
    width: 1.5rem;
    height: 1.5rem;
    border-radius: 9999px;
    background-color: #f3f4f6;
    border: 2px solid #ffffff;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 0.625rem;
    font-weight: 600;
    color: #4b5563;
    margin-right: -0.375rem;

    &--more {
      background-color: #374151;
      color: #ffffff;
    }
  }

  &__participant-count {
    margin-left: 1rem;
    font-size: 0.75rem;
    color: #9ca3af;
  }

  &__actions {
    display: flex;
    align-items: center;
    gap: 0.75rem;
  }
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.5; }
}
</style>
