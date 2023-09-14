<template>
  <div class="home">
    <HelloWorld msg="Welcome to Your Vue.js + TypeScript App" />
    <a-input-search
      v-model:value="searchParams.searchText"
      placeholder="输入搜索内容"
      enter-button="搜索"
      size="large"
      @search="onSearch"
    />
    <MyDivider />
    <a-tabs v-model:activeKey="activeKey" @change="onTabsChange">
      <a-tab-pane key="post" tab="文章">
        <PageList :page-list="posts" />
      </a-tab-pane>
      <a-tab-pane key="user" tab="用户" force-render>
        <UserList :user-list="users" />
      </a-tab-pane>
      <a-tab-pane key="picture" tab="图片">
        <pictureList :picture-list="pictures" />
      </a-tab-pane>
    </a-tabs>
  </div>
</template>

<script setup lang="ts">
import { ref } from "vue";
import PageList from "@/components/PageList.vue";
import UserList from "@/components/UserList.vue";
import pictureList from "@/components/PictureList.vue";
import MyDivider from "@/components/MyDivider.vue";
import { useRouter, useRoute } from "vue-router";
import myAxios from "@/plugins/myAxios";

const router = useRouter();
const route = useRoute();
const activeKey = ref(route.params.category);
const posts = ref([]);
const users = ref([]);
const pictures = ref([]);
var pageSize = 10;
var pageNum = 1;
const searchParams = ref({
  searchText: route.query.searchText,
  pageSize: pageSize,
  pageNum: pageNum,
});
// const initSearchOld = () => {
//   myAxios.post("/post/list/page/vo", searchParams.value).then((res: any) => {
//     posts.value = res.records;
//   });
//   const userQuery = {
//     ...searchParams,
//     userName: searchParams.value.searchText,
//   };
//   myAxios.post("/user/list/page/vo", userQuery).then((res: any) => {
//     users.value = res.records;
//   });
//   myAxios.post("/picture/list/page/vo", searchParams.value).then((res: any) => {
//     pictures.value = res.records;
//   });
// };

const initSearch = () => {
  const query = {
    ...searchParams.value,
    type: activeKey.value,
  };
  if (query.type === undefined) {
    query.type = "post";
  }
  if (query.searchText === "") {
    query.searchText = null;
  }
  myAxios.post("/search/all", query).then((res: any) => {
    if (query.type === "picture") {
      pictures.value = res.dataList;
    } else if (query.type === "user") {
      users.value = res.dataList;
    } else if (query.type === "post") {
      posts.value = res.dataList;
    }
  });
};

initSearch();

const onSearch = (value: string) => {
  console.log(value);
  router.push({
    query: searchParams.value,
  });
  initSearch();
};

const onTabsChange = (key: string) => {
  router.push({
    path: `/${key}`,
    query: searchParams.value,
  });
  initSearch();
};

// watchEffect(() => {
//   searchParams.value = {
//     searchText: route.query.searchText,
//     pageNum: pageNum,
//     pageSize: pageSize,
//   };
// });
</script>
