<template>
  <div class="el-menu-demo ep-menu">
    <div class="container flex-row flex-between">
      <div
        class="ep-sub-item title cursor-pointer"
        @click="location.href = '/'"
      >
        阿里云盘资源共享站
      </div>
      <div class="flex-row flex-item-center">
        <div class="ep-sub-item">
          <el-input
            placeholder="搜索内容"
            :prefix-icon="Search"
            v-model="keyword"
            @change="onSearch"
            clearable
          >
          </el-input>
        </div>
        <div class="ep-sub-item c-m-15 cursor-pointer" @click="dialogFormVisible = true">登录</div>
        <div class="esp-sub-item cursor-pointer" @click="dialogFormVisible = true">注册</div>
        <button
          @click="toggleDark()"
          class="esp-sub-item border-none bg-transparent cursor-pointer"
          style="height: var(--ep-menu-item-height)"
        >
          <i inline-flex i="dark:ep-moon ep-sunny" />
        </button>
      </div>
    </div>
  </div>
  <el-dialog v-model="dialogFormVisible" title="会员" width="25%">
    <el-form :model="form">
      <el-form-item label="用户名" :label-width="formLabelWidth">
        <el-input v-model="form.name" autocomplete="off" disabled />
      </el-form-item>
      <el-form-item label="密码" :label-width="formLabelWidth">
        <el-input
          type="password"
          v-model="form.password"
          autocomplete="off"
          disabled
        />
      </el-form-item>
    </el-form>
    <template #footer>
      <span class="dialog-footer">
        <el-button @click="dialogFormVisible = false">取消</el-button>
        <el-button type="primary" @click="dialogFormVisible = false">确定</el-button>
      </span>
    </template>
  </el-dialog>
</template>

<script setup>
import { ref, reactive } from "vue";
import { toggleDark } from "~/composables";
import { Search } from "@element-plus/icons-vue";

const emits = defineEmits(["search"]);
let keyword = ref("");
let dialogFormVisible = ref(false);
const formLabelWidth = "140px";
const form = reactive({
  name: "",
  password: "",
});

function onSearch() {
  emits("search", keyword.value);
}
</script>

<style lang="scss" scoped>
.el-menu-demo {
  border-bottom: 1px solid var(--ep-menu-border-color);
  min-width: 1200px;
  .title {
    font-size: 36;
    font-weight: bold;
  }
}
.c-m-15 {
  margin: 0 15px;
}
.cursor-pointer {
  margin-left: 15px;
}
.ep-sub-item {
  height: var(--ep-menu-item-height);
  line-height: var(--ep-menu-item-height);
}
</style>
