<script setup lang="ts">
import { LinkItem, isImage, isExternalLink } from '../types'

const props = defineProps<{ items: LinkItem[] }>()
</script>

<template>
  <!-- 渲染包含多个链接项的容器 -->
  <div class="container">
    <!-- 遍历 `items` 数组，渲染每个链接项 -->
    <a
      v-for="item in props.items"
      :key="item.name"
      :href="item.link"
      :title="item.name"
      :target="isExternalLink(item.link) ? '_blank' : '_self'"
      class="link"
      rel="noopener"
    >
      <!-- 渲染图标 -->
      <template v-if="item.icon">
        <img v-if="isImage(item.icon)" :src="item.icon" alt="Icon" class="icon" />
        <i v-else :class="item.icon + ' icon'" :style="{ color: item.color }"></i>
      </template>
      <template v-else>
        <img v-if="item.light" :src="item.light" alt="Icon" class="icon light-only" />
        <img v-if="item.dark" :src="item.dark" alt="Icon" class="icon dark-only" />
        <i v-if="!item.light && !item.dark" class="fas fa-arrow-up-right-from-square fa-icon" alt="Icon"></i>
      </template>

      <!-- 渲染链接项的名称和描述 -->
      <div class="text-content">
        <span class="name">{{ item.name }}</span>
        <span v-if="item.desc" class="desc">{{ item.desc }}</span>
      </div>
    </a>
  </div>
</template>

<style lang="scss" scoped>
:root:not(.dark) .dark-only,
:root:is(.dark) .light-only {
  display: none;
}

.container {
  display: flex;
  flex-wrap: wrap;
  gap: 0.5rem;
}

.link {
  width: 100%;
  height: 5rem;
  border: 1px solid var(--vp-c-bg-alt);
  background-color: var(--vp-c-bg-alt);
  border-radius: 0.8rem;
  display: flex;
  align-items: center;
  position: relative;
  transition: all 0.5s cubic-bezier(0.25, 0.8, 0.25, 1);
  text-decoration: none !important;

  &:hover {
    border-color: var(--vp-c-brand-1);
    transform: translateY(-3px);
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  }
}

.icon {
  width: 2.5rem;
  font-size: 2.5em;
  margin-left: 1.5rem;
}
.fa-icon {
  width: 2rem;
  font-size: 1.5em;
  margin-left: 2rem;
}

.text-content {
  display: flex;
  flex-direction: column;
  justify-content: flex-start;
  margin-left: 1rem;
}

.name {
  font-size: 0.875rem;
  font-weight: 600;
  line-height: 1.2;
}

.desc {
  font-size: 0.75rem;
  color: var(--vp-c-text-2);
  font-weight: 500;
  margin-top: 0.25rem;
  line-height: 1.4;
}
</style>
