<template>
  <div class="config">
    <div class="buttons">
      <Switch label="Show Title" v-model="showTitle" />
      <Switch label="Show Legend" v-model="showLegend" />
      <Switch label="Show Grid" v-model="showGrid" />
      <Switch label="Show Tooltip" v-model="showTooltip" />
      <Switch label="Show Grid Tooltip" v-model="showGridTooltip" />
    </div>
    <InputField label="Title" id="title" v-model="title" />

    <Textarea label="options" v-model="options" />

    <Button @click="saveOptions" text="OPTIONS" />
    <Button @click="saveConfig" text="Save Config" />
    <Button @click="closeOverlay" text="Cancel" />
  </div>
</template>

<script setup>
import InputField from "./InputField.vue";
import Button from "./Button.vue";
import { ref } from "vue";
import Switch from "./Switch.vue";
import Textarea from "./TextArea.vue";
import SelectField from "./SelectField.vue";

const props = defineProps({
  currentConfig: Object,
});

const title = ref(props.currentConfig.title.text);
const showTitle = ref(props.currentConfig.title.show);
const showLegend = ref(props.currentConfig.legend.show);
const showGrid = ref(props.currentConfig.grid.show);
const showTooltip = ref(props.currentConfig.tooltip.show);
const showGridTooltip = ref(props.currentConfig.grid.tooltip.show);

const options = ref("");

const emit = defineEmits(["saveConfig", "closeOverlay", "saveOptions"]);

const saveConfig = () => {
  emit("saveConfig", {
    title: title.value,
    showTitle: showTitle.value,
    showLegend: showLegend.value,
    showGrid: showGrid.value,
    showTooltip: showTooltip.value,
    showGridTooltip: showGridTooltip.value,
  });
};

const saveOptions = () => {
  emit("saveOptions", options.value);
};

const closeOverlay = () => {
  emit("closeOverlay");
};
</script>

<style lang="scss" scoped>
.config {
  height: 80%;
  width: 80%;

  background: #ececec;
  border-radius: 10px;

  padding: 10%;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  box-shadow: 0px 0px 10px 0px rgba(139, 139, 139, 0.534);

  .buttons {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    justify-content: center;
    align-items: center;
    grid-gap: 10px;
    width: 80%;
  }
}
</style>
