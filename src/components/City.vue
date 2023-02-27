<script setup lang="ts">
const props = defineProps<{
  city: String;
}>();
</script>

<template>
  <p>Current search city: {{ city }}</p>
</template>

<style lang="scss" scoped></style>
