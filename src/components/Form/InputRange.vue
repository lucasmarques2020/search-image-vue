<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  label: {
    type: String,
    required: true
  },
  min: {
    type: Number,
    default: 0
  },
  max: {
    type: Number,
    default: 5000
  },
  modelValue: {
    type: Number,
    default: 0
  }
})

const emit = defineEmits(['update:modelValue'])

const value = ref(props.modelValue)

watch(value, (newVal) => emit('update:modelValue', Number(newVal)))
watch(() => props.modelValue, (newVal) => (value.value = newVal))
</script>

<template>
  <div class="w-full flex flex-col gap-1">
    <span class="text-sm font-medium dark:text-base-content text-neutral-900">{{ label }}</span>

    <div class="flex items-center gap-1">
      <input
        type="range"
        :min="min"
        :max="max"
        v-model.number="value"
        class="range range-info range-xs flex-1"
      />

      <label class="input input-bordered flex items-center gap-1 w-32 bg-base-300 dark:bg-base-300 border-base-300 dark:text-base-content text-neutral-900">
        <input
          type="number"
          :min="min"
          :max="max"
          v-model.number="value"
          class="grow bg-transparent focus:outline-none text-right"
        />
        <span class="text-xs opacity-70">(px)</span>
      </label>
    </div>
  </div>
</template>
