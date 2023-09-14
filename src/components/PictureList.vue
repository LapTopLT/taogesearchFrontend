<template>
  <a-list
    :grid="{ gutter: 16, xs: 1, sm: 2, md: 4, lg: 4, xl: 6, xxl: 3, xxxl: 2 }"
    :data-source="$props.pictureList"
  >
    <template #renderItem="{ item }">
      <a-list-item>
        <a-card
          hoverable
          style="width: 240px"
          @click="onClickedCard(item.purl)"
        >
          <template #cover>
            <img alt="example" :src="item.url" />
          </template>
          <a-card-meta :title="item.title"> </a-card-meta>
        </a-card>
      </a-list-item>
    </template>
  </a-list>
</template>
<script setup lang="ts">
import router from "@/router";
import { withDefaults, defineProps } from "vue";
interface Props {
  pictureList: any[];
}

const props = withDefaults(defineProps<Props>(), {
  pictureList: () => [],
});

const onClickedCard = (purl: string) => {
  window.open(purl, "_self");
};
</script>
<style scoped>
.map {
  width: 1000px;
}
</style>
