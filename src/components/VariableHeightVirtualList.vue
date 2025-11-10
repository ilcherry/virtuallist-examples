<script setup>
import { ref } from "vue";
import { useVirtualList } from "@vueuse/core";

const props = defineProps({
  items: {
    type: Array,
    default: () => [],
    required: true,
  },
  containerHeight: {
    type: Number,
    default: 400,
  },
});

const container = ref(null);

// 🎯 关键：动态计算每个项的高度
const getItemHeight = (index) => {
  // 根据索引返回不同的高度
  // 示例：创建三种不同高度的项目
  const heightPattern = index % 3;
  if (heightPattern === 0) return 60; // 小项
  if (heightPattern === 1) return 80; // 中项
  return 100; // 大项
};

// itemHeight 接受函数
const { list, containerProps, wrapperProps } = useVirtualList(props.items, {
  itemHeight: getItemHeight,
  overscan: 5,
});
</script>

<template>
  <div
    ref="container"
    class="virtual-list-container"
    :style="{ height: `${containerHeight}px` }"
    v-bind="containerProps"
  >
    <div class="virtual-list-wrapper" v-bind="wrapperProps">
      <div
        v-for="item in list"
        :key="item.index"
        class="virtual-list-item"
        :style="{ height: `${getItemHeight(item.index)}px` }"
        :data-height="getItemHeight(item.index)"
      >
        <div class="item-content">
          <span class="item-index">{{ item.data.id }}</span>
          <div class="item-info">
            <span class="item-text">{{ item.data.name }}</span>
            <span class="item-height-tag">高度: {{ getItemHeight(item.index) }}px</span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.virtual-list-container {
  border: 1px solid #e8e8e8;
  border-radius: 8px;
  overflow: hidden;
  background: linear-gradient(135deg, #fafafa 0%, #f5f5f5 100%);
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.06);
}

.virtual-list-wrapper {
  width: 100%;
}

.virtual-list-item {
  width: 100%;
  border-bottom: 1px solid #f0f0f0;
  transition: background-color 0.2s ease;
  display: flex;
  align-items: center;
}

.virtual-list-item:hover {
  background-color: rgba(135, 206, 235, 0.1);
}

/* 根据高度添加不同的背景色调 */
.virtual-list-item[data-height="60"] {
  background: linear-gradient(135deg, #fff9e6 0%, #ffffff 100%);
}

.virtual-list-item[data-height="80"] {
  background: linear-gradient(135deg, #e6f7ff 0%, #ffffff 100%);
}

.virtual-list-item[data-height="100"] {
  background: linear-gradient(135deg, #f0f9ff 0%, #ffffff 100%);
}

.item-content {
  display: flex;
  align-items: center;
  padding: 0 16px;
  width: 100%;
  height: 100%;
  gap: 12px;
}

.item-index {
  display: inline-flex;
  align-items: center;
  justify-content: center;
  min-width: 28px;
  height: 28px;
  background: linear-gradient(135deg, #87ceeb 0%, #87ceeb 100%);
  color: #fff;
  border-radius: 50%;
  font-size: 12px;
  font-weight: 500;
  flex-shrink: 0;
}

.item-info {
  display: flex;
  flex-direction: column;
  gap: 4px;
  flex: 1;
  overflow: hidden;
}

.item-text {
  color: #333;
  font-size: 14px;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
  font-weight: 500;
}

.item-height-tag {
  color: #87ceeb;
  font-size: 11px;
  font-weight: 600;
  opacity: 0.8;
}
</style>

