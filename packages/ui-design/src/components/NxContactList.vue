<script setup lang="ts">
import { Search, Plus, ArrowRight, User } from "@element-plus/icons-vue";

export interface ContactItem {
    id: string;
    name: string;
    role: string;
    dept: string;
    email: string;
    avatar: any;
}

export interface DeptItem {
    name: string;
    count: number;
    icon?: any;
}

interface Props {
    title?: string;
    subtitle?: string;
    contacts: ContactItem[];
    depts: DeptItem[];
    activeDept?: string;
    searchPlaceholder?: string;
    allContactsText?: string;
    showSidebarHeader?: boolean;
    showSidebar?: boolean;
    showSearch?: boolean;
    showAdd?: boolean;
}

withDefaults(defineProps<Props>(), {
    title: "Contacts",
    subtitle: "Organization Structure",
    searchPlaceholder: "Search people...",
    activeDept: "All Contacts",
    allContactsText: "All Contacts",
    showSidebarHeader: true,
    showSidebar: true,
    showSearch: true,
    showAdd: true,
});

defineEmits<{
    (e: "select-dept", dept: string): void;
    (e: "add"): void;
    (e: "select-contact", contact: ContactItem): void;
    (e: "search", query: string): void;
}>();
</script>

<template>
  <div class="nx-contact-list">
    <!-- Sidebar: Organization -->
    <div v-if="showSidebar" class="nx-contact-sidebar">
       <div v-if="showSidebarHeader" class="nx-contact-sidebar__header">
          <h1 class="nx-contact-sidebar__title">{{ title }}</h1>
          <p class="nx-contact-sidebar__subtitle">{{ subtitle }}</p>
       </div>
       
       <div class="nx-contact-sidebar__groups">
          <div 
            v-for="dept in depts" 
            :key="dept.name" 
            class="nx-dept-item"
            :class="{ 'nx-dept-item--active': activeDept === dept.name }"
            @click="$emit('select-dept', dept.name)"
          >
             <div class="flex items-center gap-2 overflow-hidden">
                <el-icon v-if="dept.icon" class="flex-shrink-0"><component :is="dept.icon" /></el-icon>
                <el-icon v-else class="flex-shrink-0"><User /></el-icon>
                <span class="nx-dept-item__name truncate">{{ dept.name }}</span>
             </div>
             <span class="nx-dept-item__badge flex-shrink-0">{{ dept.count }}</span>
          </div>
       </div>
    </div>

    <!-- Main List -->
    <div class="nx-contact-main">
       <!-- Toolbar -->
       <div v-if="showSearch || showAdd" class="nx-contact-toolbar">
          <div v-if="showSearch" class="nx-contact-search">
             <el-icon class="nx-contact-search__icon"><Search /></el-icon>
             <input 
               type="text" 
               :placeholder="searchPlaceholder" 
               class="nx-contact-search__input"
               @input="(e: any) => $emit('search', e.target.value)"
             />
          </div>
          <div v-if="showSearch && showAdd" class="nx-contact-toolbar-divider"></div>
          <el-button v-if="showAdd" type="primary" round class="nx-contact-add-btn" @click="$emit('add')">
             <template #icon><el-icon><Plus /></el-icon></template>
             <span class="hidden sm:inline">Add Contact</span>
          </el-button>
       </div>

       <!-- Table Header -->
       <div class="nx-contact-table-header">
          <div class="nx-contact-table-col nx-contact-table-col--name">Name</div>
          <div class="nx-contact-table-col nx-contact-table-col--role hidden md:block">Role</div>
          <div class="nx-contact-table-col nx-contact-table-col--dept hidden sm:block">Department</div>
          <div class="nx-contact-table-col nx-contact-table-col--action">Action</div>
       </div>

       <!-- List -->
       <div class="nx-contact-items">
          <div 
            v-for="user in contacts" 
            :key="user.id" 
            class="nx-contact-item"
            @click="$emit('select-contact', user)"
          >
             <div class="nx-contact-table-col nx-contact-table-col--name">
                <div class="nx-contact-avatar">
                   <el-icon v-if="typeof user.avatar !== 'string'"><component :is="user.avatar" /></el-icon>
                   <span v-else>{{ user.avatar }}</span>
                </div>
                <div class="nx-contact-info overflow-hidden">
                   <div class="nx-contact-name truncate">{{ user.name }}</div>
                   <div class="nx-contact-email truncate">{{ user.email }}</div>
                </div>
             </div>
             <div class="nx-contact-table-col nx-contact-table-col--role hidden md:block">
                <span class="nx-contact-role truncate">{{ user.role }}</span>
             </div>
             <div class="nx-contact-table-col nx-contact-table-col--dept hidden sm:block">
                <span class="nx-contact-dept-tag truncate">{{ user.dept }}</span>
             </div>
             <div class="nx-contact-table-col nx-contact-table-col--action">
                <el-button circle size="small" class="nx-contact-action-btn">
                   <el-icon><ArrowRight /></el-icon>
                </el-button>
             </div>
          </div>
       </div>
    </div>
  </div>
</template>

<style lang="scss" scoped>
.nx-contact-list {
  display: flex;
  gap: 1.5rem;
  height: 100%;
  overflow: hidden;
}

.nx-contact-sidebar {
  width: 14rem;
  flex-shrink: 0;
  display: flex;
  flex-direction: column;
  gap: 1.5rem;

  &__header {
    padding-left: 0.5rem;
  }

  &__title {
    font-size: 1.25rem;
    font-weight: 700;
    color: #1a1a1a;
    margin-bottom: 0.25rem;
  }

  &__subtitle {
    font-size: 0.875rem;
    color: #909399;
  }

  &__groups {
    background-color: #ffffff;
    border-radius: 12px;
    border: 1px solid #ebeef5;
    box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.05);
    overflow-y: auto;
    padding: 8px;
    display: flex;
    flex-direction: column;
    gap: 4px;
  }
}

.nx-dept-item {
  padding: 10px 12px;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 500;
  cursor: pointer;
  display: flex;
  justify-content: space-between;
  align-items: center;
  transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);
  color: #606266;

  &:hover {
    background-color: #f5f7fa;
    color: #409eff;
  }

  &--active {
    background-color: #ecf5ff;
    color: #409eff;

    .nx-dept-item__badge {
      background-color: #d9ecff;
      color: #409eff;
    }
  }

  &__name {
    flex: 1;
  }

  &__badge {
    background-color: #f0f2f5;
    color: #909399;
    padding: 2px 6px;
    border-radius: 4px;
    font-size: 11px;
    font-weight: 600;
  }
}

.nx-contact-main {
  flex: 1;
  min-width: 0;
  background-color: #ffffff;
  border-radius: 12px;
  border: 1px solid #ebeef5;
  box-shadow: 0 2px 12px 0 rgba(0, 0, 0, 0.05);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.nx-contact-toolbar {
  padding: 12px 20px;
  border-bottom: 1px solid #f2f6fc;
  display: flex;
  align-items: center;
  gap: 12px;
  background-color: #ffffff;
}

.nx-contact-search {
  position: relative;
  flex: 1;
  max-width: 400px;
  display: flex;
  align-items: center;

  &__icon {
    position: absolute;
    left: 14px;
    top: 50%;
    transform: translateY(-50%);
    color: #909399;
    z-index: 10;
    font-size: 16px;
  }

  &__input {
    width: 100%;
    height: 38px;
    padding: 0 16px 0 42px;
    background-color: #f5f7fa;
    border: 1px solid #e4e7ed;
    border-radius: 20px;
    font-size: 14px;
    color: #303133;
    outline: none;
    transition: all 0.2s cubic-bezier(0.4, 0, 0.2, 1);

    &::placeholder {
      color: #909399;
    }

    &:focus {
      background-color: #ffffff;
      border-color: #409eff;
      box-shadow: 0 0 0 3px rgba(64, 158, 255, 0.1);
    }
  }
}

.nx-contact-toolbar-divider {
  width: 1px;
  height: 20px;
  background-color: #e4e7ed;
  margin: 0 4px;
}

.nx-contact-add-btn {
  height: 38px;
  padding: 0 20px;
  font-weight: 600;
  flex-shrink: 0;
}

.nx-contact-table-header {
  display: grid;
  grid-template-columns: 2.5fr 1.5fr 1.5fr 80px;
  padding: 12px 24px;
  background-color: #fcfcfd;
  border-bottom: 1px solid #f2f6fc;
  font-size: 11px;
  font-weight: 700;
  color: #909399;
  text-transform: uppercase;
  letter-spacing: 0.5px;

  @media (max-width: 1024px) {
    grid-template-columns: 3fr 2fr 80px;
  }

  @media (max-width: 640px) {
    grid-template-columns: 1fr 60px;
  }
}

.nx-contact-table-col {
  &--name { display: flex; align-items: center; gap: 12px; padding-right: 12px; }
  &--action { text-align: right; }
}

.nx-contact-items {
  flex: 1;
  overflow-y: auto;
  display: flex;
  flex-direction: column;

  &::-webkit-scrollbar {
    width: 6px;
  }
  &::-webkit-scrollbar-thumb {
    background: #e4e7ed;
    border-radius: 3px;
  }
}

.nx-contact-item {
  display: grid;
  grid-template-columns: 2.5fr 1.5fr 1.5fr 80px;
  padding: 14px 24px;
  align-items: center;
  cursor: pointer;
  border-bottom: 1px solid #fcfcfd;
  transition: all 0.2s ease;

  &:last-child {
    border-bottom: none;
  }

  &:hover {
    background-color: #f9fbff;

    .nx-contact-action-btn {
      opacity: 1;
      transform: translateX(0);
      background-color: #ecf5ff;
      color: #409eff;
    }
  }

  @media (max-width: 1024px) {
    grid-template-columns: 3fr 2fr 80px;
  }

  @media (max-width: 640px) {
    grid-template-columns: 1fr 60px;
  }
}

.nx-contact-avatar {
  width: 40px;
  height: 40px;
  flex-shrink: 0;
  border-radius: 10px;
  background: linear-gradient(135deg, #f5f7fa 0%, #e4e7ed 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 600;
  color: #606266;
  font-size: 14px;
  border: 1px solid #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.02);
}

.nx-contact-info {
  display: flex;
  flex-direction: column;
  gap: 2px;
  min-width: 0;
}

.nx-contact-name {
  font-weight: 600;
  color: #303133;
  font-size: 14px;
}

.nx-contact-email {
  font-size: 12px;
  color: #909399;
}

.nx-contact-role {
  font-size: 13px;
  color: #606266;
}

.nx-contact-dept-tag {
  display: inline-block;
  padding: 2px 10px;
  border-radius: 6px;
  background-color: #f0f2f5;
  color: #909399;
  font-size: 11px;
  font-weight: 600;
}

.nx-contact-action-btn {
  opacity: 0;
  transform: translateX(-4px);
  transition: all 0.25s ease;
  border: none;
  background-color: transparent;
}
</style>
