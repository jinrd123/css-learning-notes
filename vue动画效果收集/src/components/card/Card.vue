<template>
  <div class="container" @mouseleave="handleMouseLeave">
    <div
      v-for="(option, outIndex) in optionsObj"
      @mouseenter="handleMouseEnter(outIndex)"
    >
      <div class="card" :class="{ active: outIndex === activeIndex }">
        <div class="title">{{ option.title }}</div>
        <div
          class="detail"
          v-for="(item, index) in option.detail"
          :style="
            outIndex === activeIndex
              ? {
                  transform: `translate(-${86 * index}px, ${22 * index}px)`,
                }
              : null
          "
        >
          {{ item }}
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
// 实现鼠标悬浮在卡片上时一个横向排列的列表动态转换为纵向排列的列表
// 实现核心就是 通过过渡搭配:style给横向排列的列表项设置transform，必须用:style的动态样式，因为我们要借助列表项的index计算它究竟移动到哪里
import { ref } from "vue";
const optionsObj = [
  {
    title: "title1",
    detail: ["detail1-1", "detail1-2", "detail1-3"],
  },
  {
    title: "title1",
    detail: ["detail2-1", "detail2-2", "detail2-3"],
  },
  {
    title: "title1",
    detail: ["detail3-1", "detail3-2", "detail3-3"],
  },
  {
    title: "title1",
    detail: ["detail4-1", "detail4-2", "detail4-3"],
  },
];
const activeIndex = ref(0);

const handleMouseEnter = (index) => {
  activeIndex.value = index;
};
const handleMouseLeave = () => {};
</script>

<style scoped lang="less">
.card {
  transition: all 0.3s;
  background: skyblue;
  border-radius: 5px;
  margin-bottom: 5px;
  .title {
  }
  .detail {
    transition: all 0.3s;
    display: inline-block;
    margin-right: 20px;
  }
  &.active {
    background: pink;
    height: 89px;
    .detail {
      transform: translateX(100px);
    }
  }
}
</style>
