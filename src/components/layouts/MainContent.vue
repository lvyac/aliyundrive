<template>
  <div class="list">
    <div
      class="item"
      v-for="(item, index) in list"
      :key="index"
      @click="onItemClick(item)"
    >
      <div class="flex-row">
        <div class="flex-auto title text-line-2">{{ item.title }}</div>
        <div class="flex-none category flex-row" v-if="item.cat">
          <category-label :category="item.cat" />
        </div>
      </div>

      <div class="flex-row subtitle">
        <div class="">更新时间：</div>
        <div class="">{{ item.creatime }}</div>
      </div>
    </div>
  </div>
  <div class="more" v-if="showMore" v-loading="loading">
    <el-button @click="fetchData">加载更多</el-button>
  </div>

  <el-dialog v-model="dialogVisible" :title="dialogData?.title" width="30%">
    <span>{{ dialogData?.des }}</span>
    <template #footer>
      <span class="dialog-footer">
        <!-- <el-button @click="fetchUserComplaint" type="warning">举报侵权</el-button> -->
        <el-popconfirm
          title="你确定投诉该链接码?"
          @confirm="fetchUserComplaint"
          @cancel="dialogVisible = false"
        >
          <template #reference>
            <el-button>投诉</el-button>
          </template>
        </el-popconfirm>
        <a
          class="ep-button ep-button--primary"
          :href="'https://www.aliyundrive.com/s/' + dialogData?.alikey"
          target="_blank"
          @click="dialogVisible = false"
        >
          打开链接
        </a>
      </span>
    </template>
  </el-dialog>
</template>

<script setup lang="ts">
import { ref, onMounted, defineProps, watch, toRefs, reactive } from "vue";
import axios from "axios";
import { ElMessage } from "element-plus";

//axios请求拦截器
axios.interceptors.request.use(
  (config) => {
    config.headers["auth"] = "akfiwj#w6sjkjdfwiKS7WDLske";
    return config;
  },
  (error) => {
    return Promise.reject(error);
  }
);


type ListItem = {
  alikey: string;
  alititle: string;
  aliuser: string;
  badcheck: string;
  cat: string;
  creatime: string;
  des: string;
  id: number;
  title: string;
  vote: string;
};

const props = defineProps(["category", "keyword"]);
let category = props.category;
let keyword = props.keyword;
let list = reactive<ListItem[]>([]);
let offset: Number = 0;
let dialogVisible = ref<boolean>(false);
let dialogData = ref<ListItem>();
let showMore = ref<boolean>(true);
let loading = ref<boolean>(false);

onMounted(function () {
  fetchData();
});

watch(
  [() => props.category, () => props.keyword],
  ([newcategory, newkeyword]) => {
    category = newcategory;
    keyword = newkeyword;
    offset = 0;
    list = reactive<ListItem[]>([]);
    fetchData();
  }
);

const isEmpty = ref(false);

const onItemClick = (item: ListItem) => {
  dialogVisible.value = true;
  dialogData.value = item;
};

const fetchData = () => {
  let params = {
    offset,
    limit: 30,
    keyword: keyword,
    category: category.href,
  };
  loading.value = true;
  axios
    .post("https://home.lvyac.com/api/alipan", params)
    .then((res) => {
      list.push(...res.data);
        offset = offset + (res.data?.length || 0);
        if (!res.data?.length) { 
            ElMessage({
                message: "已加载全部内容",
                type: "info",
            });
            showMore.value = false;
        }
    })
    .catch((err) => {
      console.log(err);
    }).finally(() => {
      loading.value = false;
    });
};

const fetchUserComplaint = () => {
  let params = {
    id: dialogData.value?.id,
  };
  axios
    .post("https://home.lvyac.com/api/complaint", params)
    .then((res) => {
      ElMessage({
        message: "投诉成功，信息已被处理",
        type: "success",
      });
    })
    .catch((err) => {
      console.log(err);
    })
    .finally(() => {
      dialogVisible.value = false;
    });
};
</script>

<style lang="scss" scoped>
.list {
  padding: 10px 0;
  box-sizing: border-box;
  .item {
    padding: 10px 0;
    box-sizing: border-box;
    border-bottom: 1px solid var(--ep-menu-border-color);
    .title {
      font-size: 18px;
      font-weight: bold;
      color: var(--ep-text-color-primary);
    }
    .category {
      width: 120px;
      text-align: right;
      align-items: center;
      justify-content: flex-end;
    }
    .subtitle {
      font-size: 14px;
      margin-top: 10px;
      color: var(--ep-text-color-regular);
    }
  }
}
.more {
  display: flex;
  align-content: center;
  justify-content: center;
  padding-top: 15px;
  padding-bottom: 100px;
}
.ep-button {
  text-decoration: none;
}
.ep-button--primary {
  background-color: #409eff;
  border: #409eff;
}
</style>