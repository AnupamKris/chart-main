<template>
  <div class="switch-container">
    <div
      class="switch-shell"
      @click="toggleSwitch"
      :class="switchVal == true ? 'on' : ''"
    >
      <div class="switch"></div>
    </div>
    <p>{{ label }}</p>
  </div>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({
  label: String,
  modelValue: Boolean,
});

const emit = defineEmits(["update:modelValue"]);

const updateValue = (val) => emit("update:modelValue", val);

const switchVal = ref(props.modelValue);

const toggleSwitch = () => {
  switchVal.value = !switchVal.value;
  updateValue(switchVal.value);
};
</script>

<style lang="scss" scoped>
.switch-container {
  display: flex;
  align-items: center;
  margin-top: 10px;
}

p {
  padding: 0 10px;
}
.switch-shell {
  height: 30px;
  width: 60px;

  border-radius: 15px;
  position: relative;
  //   border: 1px solid #747474;
  transition: 0.3s;
  background: #c7c7c7;
  .switch {
    transition: 0.3s;
    height: 25px;
    width: 25px;
    left: 2.5px;

    border-radius: 50%;

    background: #747474;

    position: absolute;
    top: 50%;
    transform: translateY(-50%);
  }
}

.on {
  background: #0088ff;

  .switch {
    left: calc(100% - 27.5px);
    background: #ffffff;
  }
}
</style>
