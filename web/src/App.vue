<script setup lang="ts">
import { useRouteCacheStore } from '@/stores'

useHead({
  meta: [
    {
      name: 'description',
      content: 'Air724 管理面板',
    },
    {
      name: 'theme-color',
      content: () => isDark.value ? '#00aba9' : '#ffffff',
    },
  ],
  link: [
    {
      rel: 'icon',
      type: 'image/svg+xml',
      href: () => preferredDark.value ? '/favicon-dark.svg' : '/favicon.svg',
    },
  ],
})

const routeCacheStore = useRouteCacheStore()

const keepAliveRouteNames = computed(() => {
  return routeCacheStore.routeCaches
})

const mode = computed(() => {
  return isDark.value ? 'dark' : 'light'
})
</script>

<template>
  <van-config-provider :theme="mode">
    <nav-bar />
    <router-view v-slot="{ Component }">
      <section class="app-wrapper">
        <keep-alive :include="keepAliveRouteNames">
          <component :is="Component" />
        </keep-alive>
      </section>
    </router-view>
    <tab-bar />
  </van-config-provider>
</template>

<style<style scoped>
.app-wrapper {
  width: 100%;
  position: relative;
  padding: 16px;

  /* --- 新增针对 Air724 的布局修复 --- */
  /* 1. 变成固定定位，锁定在导航栏和工具栏之间 */
  position: fixed !important;
  /* 2. 这里的 top 建议与 nav-bar 的高度一致（Vant 默认通常是 46px 或 74px） */
  /* 如果你的 nav-bar 比较高，请微调这个值 */
  top: var(--van-nav-bar-height, 46px) !important; 
  left: 0;
  right: 0;
  /* 3. 核心修复：bottom 必须留出 tab-bar 的空间（Vant 默认通常是 50px） */
  bottom: var(--van-tabbar-height, 50px) !important;
  
  /* 4. 强制启用内部滚动 */
  height: auto !important;
  overflow-y: auto !important;
  -webkit-overflow-scrolling: touch !important;
  box-sizing: border-box !important;
  /* ------------------------------- */
}

/* 确保 tab-bar 始终在最前端，不被内容覆盖 */
:deep(.van-tabbar) {
  z-index: 999 !important;
}
</style>

