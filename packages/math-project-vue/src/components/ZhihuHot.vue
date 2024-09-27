<script lang="ts" setup>
import { onMounted, ref } from 'vue';
import { ZhiHuHot, ZhiHuHotList } from '../types/zhihu';

const list = ref<ZhiHuHot[]>([]);

const getData = async () => {
  /* const url =
     "https://www.zhihu.com/api/v3/feed/topstory/hot-lists/total?limit=1&desktop=true";
   let response = await fetch(url, { mode: "no-cors" });*/

  const url = "/api/v3/feed/topstory/hot-lists/total";
  let response = await fetch(url, { mode: "no-cors" });
  const res: ZhiHuHotList = await response.json();
  list.value = res.data;
};

onMounted(() => {
  getData();
});

const toggleHot = (id: number) => {
  // window.open(`https://www.zhihu.com/question/${id}`);
};
</script>

<template>
  <div class="list">
    <section v-for="(item, index) in list" :key="item.id" class="hot" @click="toggleHot(item.target.id)">
      <span>{{ index + 1 }}</span>
      <div>
        <h1 class="ellipsis_2">
          {{ item.target.excerpt }}
        </h1>
        <p class="ellipsis_1">
          {{ item.target.excerpt }}
        </p>
        <div style="text-align: start;">
          {{ item.detail_text }}
        </div>
      </div>
      <img :src="item.children[0].thumbnail" alt="">
    </section>
  </div>
</template>

<style scoped>
.list {
  display: flex;
  flex-direction: column;
  gap: 1rem;
  padding: 1rem;
}

.hot {
  display: flex;
  align-items: center;
  border: 1px solid #ffffff;
  border-radius: 8px;
  padding: 1rem;
  background-color: #e1cdbd;
  /* 淡绿色背景 */
  transition: background-color 0.3s, box-shadow 0.3s;
  cursor: pointer;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.hot:hover {
  background-color: #c68f76;
  /* 悬停时稍深的绿色 */
  box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}

.hot img {
  width: 80px;
  height: 80px;
  object-fit: cover;
  border-radius: 4px;
  margin-right: 1rem;
}

.hot div {
  flex: 1;
  word-wrap: break-word;
  /* 处理长单词或 URL */
}

.hot h1 {
  font-size: 1.2rem;
  font-weight: bold;
  margin: 0 0 0.5rem 0;
  color: #333;
  overflow: hidden;
  white-space: normal;
  /* 允许换行 */
  word-wrap: break-word;
  /* 处理长单词或 URL */
}

.hot p {
  font-size: 1rem;
  margin: 0 0 0.5rem 0;
  color: #666;
  overflow: hidden;
  white-space: normal;
  /* 允许换行 */
  word-wrap: break-word;
  /* 处理长单词或 URL */
}

.hot .detail_text {
  font-size: 0.9rem;
  color: #555;
  white-space: normal;
  /* 允许换行 */
  word-wrap: break-word;
  /* 处理长单词或 URL */
}

.ellipsis_1 {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: normal;
  /* 允许换行 */
}

.ellipsis_2 {
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: normal;
  /* 允许换行 */
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
}
</style>