<script setup>
import { ref, computed } from "vue";
import FixedHeightVirtualList from "./components/FixedHeightVirtualList.vue";
import DynamicHeightVirtualList from "./components/DynamicHeightVirtualList.vue";
import VariableHeightVirtualList from "./components/VariableHeightVirtualList.vue";
import FullyDynamicVirtualList from "./components/FullyDynamicVirtualList.vue";

// 生成 1000 条测试数据
const items = ref(
  Array.from({ length: 1000 }, (_, index) => ({
    id: index + 1,
    name: `项目 ${index + 1}`,
  }))
);

const items2 = ref(
  Array.from({ length: 4 }, (_, index) => ({
    id: index + 1,
    name: `项目 ${index + 1}`,
  }))
);

const items3 = ref(
  Array.from({ length: 400 }, (_, index) => ({
    id: index + 1,
    name: `项目 ${index + 1}`,
  }))
);

const items4 = ref(
  Array.from({ length: 200 }, (_, index) => ({
    id: index + 1,
    name: `项目 ${index + 1}`,
  }))
);
</script>

<template>
  <div class="app-container">
    <div class="header">
      <h1>✨ 虚拟列表展示</h1>
      <p class="subtitle">使用 @vueuse/core 的 useVirtualList 实现高性能列表</p>
    </div>

    <div class="content">
      <!-- 固定容器高度 -->
      <div class="section">
        <h2 class="section-title">📌 固定项高度 + 固定容器高度</h2>
        <div class="stats">
          <div class="stat-item">
            <span class="stat-label">项目高度</span>
            <span class="stat-value">50px</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">容器高度</span>
            <span class="stat-value">400px</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">数据总数</span>
            <span class="stat-value">{{ items.length }}</span>
          </div>
        </div>
        <FixedHeightVirtualList
          :items="items"
          :item-height="50"
          :container-height="400"
        />
      </div>

      <!-- 动态容器高度 -->
      <div class="section">
        <h2 class="section-title">🌊 固定项高度 + 动态容器高度</h2>
        <div class="stats">
          <div class="stat-item">
            <span class="stat-label">项目高度</span>
            <span class="stat-value">50px</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">最大高度</span>
            <span class="stat-value">400px</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">数据总数</span>
            <span class="stat-value">{{ items2.length }}</span>
          </div>
        </div>
        <DynamicHeightVirtualList
          :items="items2"
          :item-height="50"
          :max-height="400"
        />
      </div>

      <!-- 不固定项高度 + 固定容器高度 -->
      <div class="section">
        <h2 class="section-title">🎨 不固定项高度 + 固定容器高度</h2>
        <div class="stats">
          <div class="stat-item">
            <span class="stat-label">项目高度</span>
            <span class="stat-value">动态</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">容器高度</span>
            <span class="stat-value">400px</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">数据总数</span>
            <span class="stat-value">{{ items3.length }}</span>
          </div>
        </div>
        <p class="section-note">
          💡 每个项目的高度根据索引动态计算：60px / 80px / 100px 交替显示
        </p>
        <VariableHeightVirtualList :items="items3" :container-height="400" />
      </div>

      <!-- 不固定项高度 + 不固定容器高度 -->
      <div class="section">
        <h2 class="section-title">🌈 不固定项高度 + 不固定容器高度</h2>
        <div class="stats">
          <div class="stat-item">
            <span class="stat-label">项目高度</span>
            <span class="stat-value">动态</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">容器高度</span>
            <span class="stat-value">动态</span>
          </div>
          <div class="stat-item">
            <span class="stat-label">数据总数</span>
            <span class="stat-value">{{ items4.length }}</span>
          </div>
        </div>
        <p class="section-note">
          💡 项目高度动态计算（60px / 80px /
          100px），容器高度自适应（max-height: 400px）
        </p>
        <FullyDynamicVirtualList :items="items4" :max-height="400" />
      </div>
    </div>

    <div class="footer">
      <p>💡 向下滚动查看虚拟列表的强大性能</p>
    </div>
  </div>
</template>

<style scoped>
.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
  padding: 40px 20px;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", "Roboto", "Oxygen",
    "Ubuntu", "Cantarell", "Fira Sans", "Droid Sans", "Helvetica Neue",
    sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 40px;
}

.header h1 {
  font-size: 32px;
  color: #2c3e50;
  margin: 0 0 12px 0;
  font-weight: 600;
}

.subtitle {
  font-size: 16px;
  color: #7f8c8d;
  margin: 0;
  font-weight: 400;
}

.content {
  max-width: 1600px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 24px;
}

@media (max-width: 1200px) {
  .content {
    grid-template-columns: 1fr;
  }
}

.section {
  background: white;
  padding: 24px;
  border-radius: 12px;
  box-shadow: 0 4px 16px rgba(0, 0, 0, 0.08);
}

.section-title {
  font-size: 16px;
  color: #2c3e50;
  margin: 0 0 12px 0;
  font-weight: 600;
}

.section-note {
  background: linear-gradient(135deg, #f0f9ff 0%, #e0f2fe 100%);
  padding: 10px 12px;
  border-radius: 6px;
  border-left: 3px solid #87ceeb;
  color: #2563eb;
  font-size: 12px;
  margin: 0 0 12px 0;
  line-height: 1.4;
}

.stats {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
  margin-bottom: 16px;
}

.stat-item {
  background: #f8fafc;
  padding: 12px;
  border-radius: 6px;
  text-align: center;
  box-shadow: 0 1px 4px rgba(0, 0, 0, 0.06);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.stat-item:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.12);
}

.stat-label {
  display: block;
  font-size: 11px;
  color: #95a5a6;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 6px;
}

.stat-value {
  display: block;
  font-size: 20px;
  color: #87ceeb;
  font-weight: 600;
}

.footer {
  text-align: center;
  margin-top: 32px;
  color: #7f8c8d;
  font-size: 14px;
}
</style>
