<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  modelValue: {
    type: String,
    default: 'webp'
  },
  label: {
    type: String,
    default: 'Formato da imagem'
  }
})

const emit = defineEmits(['update:modelValue'])
const selected = ref(props.modelValue)

watch(selected, (val) => emit('update:modelValue', val))
watch(() => props.modelValue, (val) => (selected.value = val))
</script>

<template>
  <div class="w-full flex flex-col gap-1">
    <span class="text-sm font-medium dark:text-base-content text-neutral-900">{{ label }}</span>
    <select 
      v-model="selected" 
      class="select select-bordered w-full bg-base-300 dark:bg-base-300 border-base-300 dark:text-base-content text-neutral-900"
    >
      <option value="webp">WebP (Recomendado)</option>
      <option value="jpg">JPG</option>
      <option value="png">PNG</option>
    </select>
  </div>
</template>