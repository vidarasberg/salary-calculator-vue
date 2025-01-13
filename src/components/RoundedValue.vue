<script setup lang="ts">
defineProps({
  label: { type: String, required: true },
  value: { type: Number, required: true },
  fractionDigits: { type: Number },
  suffix: { type: String },
})
</script>

<template>
  <div>{{ label }}: {{ value.toFixed(fractionDigits) }} {{ suffix }}</div>
</template>
