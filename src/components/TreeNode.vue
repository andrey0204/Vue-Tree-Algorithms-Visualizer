<template>
  <div class="flex flex-col items-center">
    <!-- Nodo actual -->
    <div
      class="rounded-full w-12 h-12 flex items-center justify-center border text-xl transition-all duration-300"
      :class="{
        'bg-green-400 scale-110 font-bold': node.id === activeId,
        'bg-red-400 scale-110 font-bold': node.id === errorId,
        'bg-gray-100': node.id !== activeId && node.id !== errorId
      }"
    >
      {{ node.value }}
    </div
    <!-- Conexiones -->
    <div class="flex gap-38 mt-2" v-if="node.left || node.right">
      <div v-if="node.left" class="w-0 h-6 border-l border-black translate-x-6"></div>
      <div v-if="node.right" class="w-0 h-6 border-l border-black -translate-x-6"></div>
    </div>

    <!-- Hijos -->
    <div class="flex justify-center gap-16 mt-2">
      <TreeNode v-if="node.left" :node="node.left" :activeId="activeId" />
      <TreeNode v-if="node.right" :node="node.right" :activeId="activeId" />
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  node: {
    id: number
    value: string
    left?: any
    right?: any
  },
  activeId: number | null
  errorId?: number | null
}>()
</script>
