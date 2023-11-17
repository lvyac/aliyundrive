<template>
  <el-config-provider namespace="ep">
    <el-affix :offset="0">
      <header-nav @search="searchChange"></header-nav>
    </el-affix>
    
    <div class="flex-row main-container">
      <div class="side">
        <base-side @change="categoryChange"></base-side>
      </div>
      <div class="main">
        <div class="" v-if="keyword" style="background-color: red;">
          <div class="banner other">搜索【{{ keyword }}】相关内容</div>
        </div>
        <div class="banner flex-row" v-else-if="category.href" :style="{backgroundColor: category.color, color: '#fff'}">
          <div class="flex-row flex-item-center">
            <el-icon size="32"><component :is="category.icon"></component></el-icon>
            <div style="margin-left: 5px;">{{ category.name }}</div>
          </div> 
        </div>
        <div class="banner other" v-else>
          <div>声明：本站所有内容均来自阿里网盘分享</div>
          <div>如有侵权，请联系我删除</div>
        </div>
        <main-content :category="category" :keyword="keyword"></main-content>
      </div>
      
    </div>
    <el-backtop :bottom="100">
    <div
      style="
        height: 100%;
        width: 100%;
        background-color: var(--el-bg-color-overlay);
        box-shadow: var(--el-box-shadow-lighter);
        text-align: center;
        line-height: 60px;
        color: #1989fa;
      "
    >
      <el-icon size="32"><Top /></el-icon>
    </div>
  </el-backtop>
  </el-config-provider>
</template>

<script setup lang="ts">

import { onMounted, reactive, ref, shallowRef, ComponentOptions, toRefs } from 'vue'
import { Top } from "@element-plus/icons-vue";


type categoryData = {
  name: String,
  color: String,
  cat: String,
  href?: String | string | "",
  icon: Object | ComponentOptions
}


let keyword = ref('')
let category = shallowRef<categoryData>({ name: "", color: "#ffff", cat: "", href: "", icon: {} })


const searchChange = (_keyword: string) => { 
  keyword.value = _keyword
}


const categoryChange = (_category: categoryData) => { 
  category.value = _category
}


</script>

<style lang="scss">
#app {

  color: var(--ep-text-color-primary);
}

.main-container {
  width: 1200px;
  margin-top: 0;
  margin-left: auto;
  margin-right: auto;
  .side{
    padding-right: 15px;
    box-sizing: border-box;
    flex: none;
  }
  .main{
    padding-left: 15px;
    flex: auto;
    box-sizing: border-box;
  }
}
.banner{
  justify-content: center;
  height: 100px;
  font-size: 32px;
  margin-top: 15px;
  align-items: center;
  text-align: center;
  background-color: #E8ECF3;
}
.other{
  font-size: 18px;
  color: var(--ep-text-color-regular);
  display: flex;
  flex-direction: column;
}

</style>
