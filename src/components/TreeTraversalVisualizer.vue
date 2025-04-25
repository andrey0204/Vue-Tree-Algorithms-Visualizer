<template>
  <div class="p-4">
    <h2 class="text-lg font-bold mb-4">🔁 Recorrido de Árbol Binario</h2>

    <div class="flex flex-col items-center mb-6 gap-2">
      <el-radio-group v-model="selectedOrder" size="small">
        <el-radio-button label="preorder">Preorden</el-radio-button>
        <el-radio-button label="inorder">Inorden</el-radio-button>
        <el-radio-button label="postorder">Postorden</el-radio-button>
      </el-radio-group>
      <p class="text-sm text-gray-600 italic mb-4">{{ traversalDescription }}</p>

      <button
        @click="startTraversal"
        class="bg-blue-500 text-white px-4 py-1 rounded shadow hover:bg-blue-600 transition"
      >
        Iniciar Recorrido
      </button>
    </div>

    <div class="flex justify-center items-center gap-6 mt-6">
      <TreeNode
        :node="tree"
        :activeId="currentId"
      />
    </div>

    <div class="mt-4 text-sm space-y-1">
      <p>🔢 Paso: {{ step }}</p>
      <p>🕒 Tiempo total: <span class="text-blue-600 font-semibold">{{ elapsedTime }} ms</span></p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref, computed } from 'vue'
import TreeNode from './TreeNode.vue' // Componente que renderiza visualmente el árbol

type Tree = {
  id: number
  value: string
  left?: Tree
  right?: Tree
}

const tree: Tree = {
  id: 1,
  value: 'A',
  left: {
    id: 2,
    value: 'B',
    left: { id: 4, value: 'D' },
    right: { id: 5, value: 'E' }
  },
  right: {
    id: 3,
    value: 'C',
    right: { id: 6, value: 'F' }
  }
}

const selectedOrder = ref<'preorder' | 'inorder' | 'postorder'>('inorder')
const currentId = ref<number | null>(null)
const step = ref(0)
const elapsedTime = ref<string>('0')

const sleep = (ms: number) => new Promise(resolve => setTimeout(resolve, ms))

async function traverse(tree: Tree | undefined, order: typeof selectedOrder.value, visit: (id: number) => Promise<void>) {
  if (!tree) return
  if (order === 'preorder') await visit(tree.id)
  await traverse(tree.left, order, visit)
  if (order === 'inorder') await visit(tree.id)
  await traverse(tree.right, order, visit)
  if (order === 'postorder') await visit(tree.id)
}

async function startTraversal() {
  step.value = 0
  const start = performance.now()

  await traverse(tree, selectedOrder.value, async (id) => {
    currentId.value = id
    step.value++
    await sleep(500)
  })

  const end = performance.now()
  elapsedTime.value = (end - start).toFixed(2)
  currentId.value = null
}

const traversalDescription = computed(() => {
  switch (selectedOrder.value) {
    case 'preorder':
      return '🔷 Preorden: Visita primero la raíz, luego el subárbol izquierdo y finalmente el derecho. → A → B → D → E → C → F'
    case 'inorder':
      return '🔶 Inorden: Visita primero el subárbol izquierdo, luego la raíz y finalmente el derecho. → D → B → E → A → C → F'
    case 'postorder':
      return '🔸 Postorden: Visita primero el subárbol izquierdo, luego el derecho y finalmente la raíz. → D → E → B → F → C → A'
    default:
      return ''
  }
})
</script>
