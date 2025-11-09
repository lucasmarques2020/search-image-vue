<script setup>
import { ref, watch } from 'vue'

const props = defineProps({
  label: {
    type: String,
    required: true
  },
  placeholder: {
    type: String,
    default: ''
  },
  modelValue: {
    type: String,
    default: ''
  }
})

const emit = defineEmits(['update:modelValue'])
const value = ref(props.modelValue)

watch(value, (newVal) => emit('update:modelValue', newVal))
watch(() => props.modelValue, (newVal) => (value.value = newVal))

</script>

<template>
  <div class="w-full flex flex-col gap-1">
    <span class="text-sm font-medium dark:text-base-content text-neutral-900">{{ label }}</span>

    <label class="input input-bordered flex items-center gap-2 bg-base-300 dark:bg-base-300 border-base-300 dark:text-base-content text-neutral-900">
      <input
        type="text"
        v-model="value"
        :placeholder="placeholder"
        class="grow bg-transparent focus:outline-none dark:placeholder-base-content/50 placeholder-neutral-500"
      />
    </label>
  </div>
</template>
