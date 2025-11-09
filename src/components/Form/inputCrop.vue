<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  modelValue: {
    type: String,
    default: 'square'
  },
  label: {
    type: String,
    default: 'Tipo de máscara'
  }
})

const emit = defineEmits(['update:modelValue'])
const selected = ref(props.modelValue)

watch(selected, (val) => emit('update:modelValue', val))
watch(() => props.modelValue, (val) => (selected.value = val))
</script>

<template>
  <div class="flex flex-col gap-2 w-full">
    <label class="text-sm font-medium dark:text-base-content text-neutral-900 mb-1">{{ label }}</label>

    <div class="flex items-center justify-center gap-4 bg-base-200 p-3 rounded-lg border border-base-300">
      <label title="Sem shape" class="cursor-pointer relative group">
        <input
          type="radio"
          name="crop"
          value="none"
          v-model="selected"
          class="hidden peer"
        />
        <div
          class="w-12 h-12 border-2 dark:border-gray-700 border-neutral-400 peer-checked:border-info hover:border-info transition-all duration-200 flex items-center justify-center dark:bg-base-300 bg-neutral-100"
        >
          <svg class="w-8 h-8 dark:text-gray-100 text-neutral-600 peer-checked:text-info" viewBox="0 0 24 24" fill="none" stroke="currentColor">
            <rect x="4" y="4" width="16" height="16" stroke-width="2"/>
            <line x1="7" y1="7" x2="17" y2="17" stroke-width="2"/>
            <line x1="17" y1="7" x2="7" y2="17" stroke-width="2"/>
          </svg>
        </div>
      </label>

      <label title="Corte quadrado" class="cursor-pointer relative group">
        <input
          type="radio"
          name="crop"
          value="square"
          v-model="selected"
          class="hidden peer"
        />
        <div
          class="w-12 h-12 border-2 border-base-content/20 rounded-md peer-checked:border-info hover:border-info transition-all duration-200 flex items-center justify-center bg-base-300"
        >
          <div class="w-8 h-8 bg-gray-100 rounded-sm peer-checked:bg-info"></div>
        </div>
      </label>

      <label title="Corte redondo" class="cursor-pointer relative group">
        <input
          type="radio"
          name="crop"
          value="circle"
          v-model="selected"
          class="hidden peer"
        />
        <div
          class="w-12 h-12 border-2 border-base-content/20 rounded-full peer-checked:border-info hover:border-info transition-all duration-200 flex items-center justify-center bg-base-300"
        >
          <div class="w-8 h-8 bg-gray-100 rounded-full peer-checked:bg-info"></div>
        </div>
      </label>
    </div>
  </div>
</template>
