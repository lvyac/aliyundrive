<template>
    <div class="flex-row flex-item-center label" :style="{backgroundColor: data.color}">
        <el-icon>
            <component :is="data.icon" />
        </el-icon>
        <div class="name">{{ data.name }}</div>
    </div>
</template>

<script setup>

import { defineProps, ref, shallowRef } from 'vue'
import category from "~/composables/category"
const props = defineProps(['category'])
let data = shallowRef(category.qt)

if (props.category && category[props.category]) { 
    data = shallowRef(category[props.category])
}
</script>

<style lang="scss" scoped>
.label{
    font-size: 12px;
    border-radius: 4px;
    color: #fff;
    padding: 2px 5px;
    box-sizing: border-box;
    .name{
        margin-left: 3px;
    }
}
</style>