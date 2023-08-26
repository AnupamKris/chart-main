<template>
  <div class="input-field">
    <input
      type="text"
      :value="modelValue"
      @input="updateValue"
      :id="id"
      placeholder=" "
    />
    <label :for="id">{{ label }}</label>
  </div>
</template>

<script setup>
const props = defineProps({
  modelValue: String,
  label: String,
  id: String,
});

const emit = defineEmits(["update:modelValue"]);

const updateValue = (event) => emit("update:modelValue", event.target.value);
</script>

<style lang="scss" scoped>
.input-field {
  position: relative;
  width: 80%;
  height: 50px;
  margin-top: 15px;
  background: inherit;

  input {
    height: 100%;
    width: 100%;

    outline: none;
    border: 1px solid #727272;
    background: transparent;
    padding: 0 15px;
    border-radius: 10px;

    &:focus + label,
    &:not(:placeholder-shown) + label {
      top: 0;
    }
  }

  label {
    position: absolute;
    top: 50%;
    left: 15px;

    transform: translateY(-50%);

    display: flex;
    align-items: center;

    padding: 0 10px;

    pointer-events: none;
    color: #727272;
    background: inherit;
    transition: 0.2s;
  }
}
</style>
