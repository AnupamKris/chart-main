<template>
  <button>{{ text }}</button>
</template>

<script setup>
defineProps({
  text: String,
});
</script>

<style lang="scss" scoped>
button {
  height: 50px;
  width: 80%;
  border-radius: 10px;
  border: none;
  outline: none;
  background: #e2e2e2;

  margin-top: 15px;
}
</style>
