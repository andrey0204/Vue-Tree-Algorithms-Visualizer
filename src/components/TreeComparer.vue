<template>
  <div class="p-4">
    <h2 class="text-lg font-bold mb-4">🔍 Comparar dos Árboles Binarios</h2>

    <div class="flex justify-center gap-4 mb-4">
      <button @click="generateRandomTrees" class="bg-indigo-500 text-white px-3 py-1 rounded hover:bg-indigo-600">
        Generar árboles aleatorios
      </button>
      <button @click="startComparison" class="bg-blue-500 text-white px-3 py-1 rounded hover:bg-blue-600">
        Comparar Árboles
      </button>
    </div>

    <div class="flex justify-around gap-8 mt-4">
      <div class="flex flex-col items-center">
        <p class="font-semibold mb-2">🌲 Árbol 1</p>
        <TreeNode :node="tree1" :activeId="current1" :errorId="errorId1" />
      </div>
      <div class="flex flex-col items-center">
        <p class="font-semibold mb-2">🌳 Árbol 2</p>
        <TreeNode :node="tree2" :activeId="current2" :errorId="errorId2" />
      </div>
    </div>

    <div class="mt-6 text-sm space-y-1 text-center">
      <p>🔁 Paso: {{ step }}</p>
      <p>🕒 Tiempo total: <span class="text-blue-600 font-semibold">{{ elapsedTime }} ms</span></p>
      <p v-if="result !== null" class="font-bold text-xl mt-2" :class="{
        'text-green-600': result,
        'text-red-500': !result
      }">
        {{ result ? '✅ Los árboles son iguales' : '❌ Los árboles son diferentes' }}
      </p>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import TreeNode from './TreeNode.vue'

type Tree = {
  id: number
  value: string
  left?: Tree
  right?: Tree
}

function randomValue() {
  return String.fromCharCode(65 + Math.floor(Math.random() * 6)) // A–F
}

function generateTree(idStart = 1): Tree {
  return {
    id: idStart,
    value: randomValue(),
    left: {
      id: idStart + 1,
      value: randomValue(),
      left: { id: idStart + 3, value: randomValue() },
      right: { id: idStart + 4, value: randomValue() }
    },
    right: {
      id: idStart + 2,
      value: randomValue(),
      right: { id: idStart + 5, value: randomValue() }
    }
  }
}

const tree1 = ref<Tree>(generateTree())
const tree2 = ref<Tree>(generateTree(10))

const current1 = ref<number | null>(null)
const current2 = ref<number | null>(null)
const errorId1 = ref<number | null>(null)
const errorId2 = ref<number | null>(null)

const step = ref(0)
const elapsedTime = ref<string>('0')
const result = ref<boolean | null>(null)

const sleep = (ms: number) => new Promise(resolve => setTimeout(resolve, ms))

async function compareTrees(t1: Tree | undefined, t2: Tree | undefined): Promise<boolean> {
  step.value++

  if (!t1 && !t2) return true
  if (!t1 || !t2) {
    if (t1) errorId1.value = t1.id
    if (t2) errorId2.value = t2.id
    return false
  }

  current1.value = t1.id
  current2.value = t2.id
  await sleep(500)

  if (t1.value !== t2.value) {
    errorId1.value = t1.id
    errorId2.value = t2.id
    return false
  }

  const leftEqual = await compareTrees(t1.left, t2.left)
  if (!leftEqual) return false

  const rightEqual = await compareTrees(t1.right, t2.right)
  return rightEqual
}

async function startComparison() {
  step.value = 0
  current1.value = current2.value = errorId1.value = errorId2.value = null
  result.value = null

  const start = performance.now()
  result.value = await compareTrees(tree1.value, tree2.value)
  const end = performance.now()

  elapsedTime.value = (end - start).toFixed(2)
  current1.value = current2.value = null
}

function generateRandomTrees() {
  tree1.value = generateTree()
  tree2.value = generateTree(10)
  current1.value = current2.value = errorId1.value = errorId2.value = null
  result.value = null
  step.value = 0
  elapsedTime.value = '0'
}
</script>
