<template>
  <div class="flex flex-col items-center">
    <div
      class="rounded-full w-12 h-12 flex items-center justify-center border text-xl mb-2 cursor-pointer transition-all"
      :class="{
        'bg-green-400 font-bold scale-110': node.id === selectedId,
        'bg-gray-100 hover:bg-gray-200': node.id !== selectedId
      }"
      @click="$emit('select', node.id)"
    >
      {{ node.label }}
    </div>

    <div v-if="node.child" class="flex flex-col items-center">
      <div class="w-0 h-6 border-l border-black mb-1"></div>
      <NaryNodeView :node="node.child" :selectedId="selectedId" @select="$emit('select', $event)" />
    </div>

    <div v-if="node.next" class="flex items-start gap-4 mt-2">
      <NaryNodeView :node="node.next" :selectedId="selectedId" @select="$emit('select', $event)" />
    </div>
  </div>
</template>

<script setup lang="ts">
defineProps<{
  node: {
    id: number
    label: string
    child?: any
    next?: any
  },
  selectedId: number | null
}>()

defineEmits(['select'])
</script>
