<template>
  <div class="sidebar-container v-scrollbar">
    <div class="logo-bannner">
      <img src="./images/banner.png" alt="">
    </div>
    <el-menu
      :default-active="activeMenu"
      text-color="#969696"
      active-text-color="#2d8cf0"
      style="border-right: none;"
      router>
      <template v-for="(item, index) in menus[0].children" :key="index">
        <el-sub-menu v-if="item.children" :index="item.path">
          <template #title>
            <el-icon><component :is="item.meta.icon"></component></el-icon>
            <span>{{ item.meta.title }}</span>
          </template>
          <template v-for="(subItem, subIndex) in item.children" :key="subIndex">
            <el-menu-item
              v-if="!subItem.meta.hidden"
              :index="subItem.path">
              <el-icon><component :is="subItem.meta.icon"></component></el-icon>
              <span>{{ subItem.meta.title }}</span>
            </el-menu-item>
          </template>
        </el-sub-menu>
        <el-menu-item v-else-if="!item.meta.hidden" :index="item.path">
          <el-icon><component :is="item.meta.icon"></component></el-icon>
          <span>{{ item.meta.title }}</span>
        </el-menu-item>
      </template>
    </el-menu>
  </div>
</template>

<script lang="ts" setup>
import useUserStore from '@/store/user';
import { computed, ref, watch } from 'vue';
import { useRoute } from 'vue-router';

/**
 * 实例
 */
const route = useRoute()
const userStore = useUserStore()

/**
 * 变量
 */
const activeMenu: any = ref('/')
const menus = computed(() => {
  return userStore.user_menus
})

/**
 * 监听器
 */
watch(() => route, (val) => {
  const { meta, path, name } = val
  if (meta && meta.active) {
    activeMenu.value = meta.active
  } else {
    activeMenu.value = path
  }
}, {
  immediate: true,
  deep: true
})
</script>

<style lang="scss">
.sidebar-container {
  padding: 20px 10px;
  .logo-bannner {
    width: 100%;
    img {
      width: 100%;
    }
  }
  .el-menu-item, .el-sub-menu__title {
    // height: 45px;
    // line-height: 45px;
    // margin: 5px 0;
    &.is-active {
      background: #f0faff;
      border-radius: 4px;
    }
    &:not(.is-active):hover {
      background: none;
    }
  }
}
</style>