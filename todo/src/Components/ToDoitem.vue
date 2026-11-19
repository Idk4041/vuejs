<script setup>
const props = defineProps({ todo: Object })
const emit = defineEmits(['toggle', 'remove'])

const colors = {
  Hoog: 'bg-red-100 text-red-700',
  Middel: 'bg-yellow-100 text-yellow-700',
  Laag: 'bg-green-100 text-green-700',
}
</script>

<template>
  <li class="flex items-center gap-3 rounded-lg bg-white p-3 shadow-sm">
    <input
      type="checkbox"
      :checked="todo.done"
      @change="emit('toggle', todo.id)"
      class="h-5 w-5"
    />
    <span
      class="flex-1"
      :class="todo.done ? 'text-slate-400 line-through' : 'text-slate-800'"
    >
      {{ todo.text }}
    </span>
    <span class="rounded-full px-2 py-0.5 text-xs font-medium" :class="colors[todo.priority]">
      {{ todo.priority }}
    </span>
    <button
      @click="emit('remove', todo.id)"
      class="rounded bg-red-500 px-3 py-1 text-sm text-white hover:bg-red-600"
    >
      Verwijderen
    </button>
  </li>
</template>