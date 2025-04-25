<template>
  <div class="max-w-6xl mx-auto p-6">
    <h1 class="text-2xl font-bold text-center mb-6">🌳 Visualizador de Árboles</h1>

    <el-tabs v-model="activeTab" type="border-card" class="mb-4 justify-center">
      <el-tab-pane label="🔁 Recorridos (O(n))" name="traversal">
        <TreeTraversalVisualizer />
      </el-tab-pane>

      <el-tab-pane label="🔍 Comparar Árboles" name="comparer">
        <TreeComparer />
      </el-tab-pane>

      <el-tab-pane label="🌐 Representación N-aria" name="renderer">
        <TreeRenderer />
      </el-tab-pane>
    </el-tabs>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

// Importar los componentes (puedes dejarlos vacíos por ahora y crearlos luego)
import TreeTraversalVisualizer from './TreeTraversalVisualizer.vue'
import TreeComparer from './TreeComparer.vue'
import TreeRenderer from './TreeRenderer.vue'

const activeTab = ref('traversal')
</script>
