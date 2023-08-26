<template>
  <div class="config-overlay">
    <slot></slot>
  </div>
</template>

<script setup></script>

<style lang="scss" scoped>
.config-overlay {
  height: 100%;
  width: 100%;

  background: #c4c4c455;
  backdrop-filter: blur(10px);

  position: fixed;
  top: 0;
  left: 0;

  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
