<template>
  <div class="p-4">
    <h2 class="text-lg font-bold mb-4">🌐 Árbol N-ario Dinámico (child / next)</h2>
    <div class="mb-6 text-center">
      <div class="font-mono text-sm leading-snug inline-block px-4 py-2 bg-gray-50 border rounded shadow-sm">
        A
        <br>│
        <br>B ───► C ───► D
        <br>│
        <br>E
      </div>
      <p class="text-xs text-gray-600 mt-2 italic max-w-md mx-auto">
        En esta representación, cada nodo tiene un solo hijo (↓) y hermanos (→).
        Se visualiza como un árbol binario donde <code>child</code> apunta al primer hijo y <code>next</code> a los hermanos.
      </p>
    </div>
    <p class="text-sm text-gray-600 italic mb-4">
      Haz clic en un nodo para seleccionarlo y luego usa el botón para agregar un hijo.
    </p>

    <div class="mb-4 flex gap-4">
      <input
        v-model="newLabel"
        placeholder="Etiqueta del nuevo nodo"
        class="border rounded px-2 py-1"
      />
      <button
        @click="addNode"
        class="bg-green-500 text-white px-3 py-1 rounded hover:bg-green-600"
      >
        ➕ Agregar Hijo
      </button>
    </div>

    <div class="flex justify-center mt-6">
      <NaryNodeView :node="naryTree" :selectedId="selectedId" @select="selectNode" />
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import NaryNodeView from './NaryNodeView.vue'

type NaryNode = {
  id: number
  label: string
  child?: NaryNode
  next?: NaryNode
}

const naryTree = ref<NaryNode>({
  id: 1,
  label: 'A'
})

let nodeIdCounter = 2
const selectedId = ref<number | null>(null)
const newLabel = ref('')

function selectNode(id: number) {
  selectedId.value = id
}

function findNodeById(node: NaryNode, id: number): NaryNode | null {
  if (node.id === id) return node
  if (node.child) {
    const found = findNodeById(node.child, id)
    if (found) return found
  }
  if (node.next) {
    const found = findNodeById(node.next, id)
    if (found) return found
  }
  return null
}

function addNode() {
  if (!newLabel.value.trim() || selectedId.value === null) return

  const parent = findNodeById(naryTree.value, selectedId.value)
  if (!parent) return

  const newNode: NaryNode = {
    id: nodeIdCounter++,
    label: newLabel.value.trim()
  }

  if (!parent.child) {
    parent.child = newNode
  } else {
    let sibling = parent.child
    while (sibling.next) {
      sibling = sibling.next
    }
    sibling.next = newNode
  }

  newLabel.value = ''
}
</script>
