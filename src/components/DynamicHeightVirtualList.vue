<script setup>
import { ref } from "vue";
import { useVirtualList } from "@vueuse/core";

const props = defineProps({
  items: {
    type: Array,
    default: () => [],
    required: true,
  },
  itemHeight: {
    type: Number,
    default: 50,
  },
  maxHeight: {
    type: Number,
    default: 500,
  },
});

const container = ref(null);

const { list, containerProps, wrapperProps } = useVirtualList(props.items, {
  itemHeight: props.itemHeight,
  overscan: 5,
});
</script>

<template>
  <div
    ref="container"
    class="virtual-list-container"
    :style="{ maxHeight: `${maxHeight}px` }"
    v-bind="containerProps"
  >
    <div class="virtual-list-wrapper" v-bind="wrapperProps">
      <div
        v-for="item in list"
        :key="item.index"
        class="virtual-list-item"
        :style="{ height: `${itemHeight}px` }"
      >
        <div class="item-content">
          <span class="item-index">{{ item.data.id }}</span>
          <span class="item-text">{{ item.data.name }}</span>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.virtual-list-container {
  border: 1px solid #e8e8e8;
  border-radius: 8px;
  overflow-y: auto;
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

.item-text {
  color: #333;
  font-size: 14px;
  flex: 1;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
</style>
