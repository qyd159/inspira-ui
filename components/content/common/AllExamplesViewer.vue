<template>
  <div class="examples-container">
    <div
      v-for="(example, index) in examples"
      :key="index"
      class="example-item"
    >
      <component :is="example.component" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { defineAsyncComponent } from "vue";

const examples = [
  {
    name: "AnimatedBeamDemo",
    component: defineAsyncComponent(
      () => import("@/components/content/inspira/examples/AnimatedBeamDemo.vue"),
    ),
  },
  {
    name: "AnimatedListDemo",
    component: defineAsyncComponent(
      () => import("@/components/content/inspira/examples/AnimatedListDemo.vue"),
    ),
  },
  // 其他组件按相同格式添加...
];

// 动态加载所有组件
async function loadAllExamples() {
  const modules = import.meta.glob("@/components/content/inspira/examples/**/*Demo.vue");
  for (const path in modules) {
    const componentName = path.split("/").pop()?.replace(".vue", "") || "";
    examples.push({
      name: componentName,
      component: defineAsyncComponent(modules[path]),
    });
  }
}

loadAllExamples();
</script>

<style scoped>
.examples-container {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 20px;
  padding: 20px;
}

.example-item {
  break-inside: avoid;
  margin-bottom: 20px;
}
</style>
