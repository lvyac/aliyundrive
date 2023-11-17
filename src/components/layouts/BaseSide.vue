<template>
  <el-menu
    default-active="0"
    class="el-menu-vertical-demo no-boder"
    :collapse="isCollapse"
    @close="handleClose"
    @select="handleOpen"
  >
    
    <el-menu-item v-for="(item, _index) in menuList" :key="_index" :index="_index.toString()">
      <el-icon :style="{color: item.color}"><component :is="item.icon"></component></el-icon>
      <template #title>{{ item.name }}</template>
    </el-menu-item>
  </el-menu>
</template>

<script lang="ts" setup>
import { ref, shallowRef } from "vue";
import {
  VideoCamera,
  Film,
  Lollipop,
  Camera,
  Service,
  ShoppingBag,
  Document as IconDocument,
  Menu as IocnMenu,
  Picture,
  Setting as IconMore
} from "@element-plus/icons-vue";

const isCollapse = ref(false);
const menuList = shallowRef([
  { id: 99, name: "全部", icon: IocnMenu, href: null, color: "#4D698E" },
  { id: 1, name: "电影", icon: Film, href: "hydy", color: "#EE3525" },
  { id: 2, name: "电视", icon: VideoCamera, href: "hyds", color: "#FED43C" },
  { id: 3, name: "动漫", icon: Lollipop, href: "hydm", color: "#9CCA49" },
  { id: 4, name: "视频", icon: Camera, href: "jlp", color: "#4DABF7" },
  { id: 5, name: "音乐", icon: Service, href: "hyyy", color: "#FD7E13" },
  { id: 6, name: "软件", icon: ShoppingBag, href: "kfrj", color: "#15AABF" },
  { id: 7, name: "文档", icon: IconDocument, href: "mh", color: "#4C6EF5" },
  { id: 8, name: "图片", icon: Picture, href: "bz", color: "#FED43C" },
  { id: 9, name: "其他", icon: IconMore, href: "qt", color: "#E8ECF3" }
]);

const emits = defineEmits(['change'])

const handleOpen = (key: string, keyPath: string[]) => {
  emits('change', menuList.value[parseInt(key)])
};
const handleClose = (key: string, keyPath: string[]) => {
  console.log(key, keyPath);
};

</script>
