<script setup lang="ts">
const props = defineProps({
  label: { type: String, required: true },
  id: { type: String, required: true },
})
const modelValue = defineModel<number>({ required: true })
</script>

<template>
  <div>
    <label :for="props.id">{{ props.label }}</label>
    <input type="number" :id="props.id" v-model="modelValue" />
  </div>
</template>
