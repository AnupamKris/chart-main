<template>
  <div class="axis-config">
    <div class="buttons">
      <Switch label="Show Axis" v-model="showAxis" />
    </div>
    <InputField label="Axis Name" v-model="axisName" />
    <SelectField
      label="Axis Type"
      :options="['category', 'value', 'time', 'log']"
      id="x-axis-type"
      v-model="axisType"
    />
    <SelectField
      label="Axis Name Location"
      :options="['start', 'center', 'end']"
      id="x-axis-name-location"
      v-model="axisNameLocation"
    />
    <InputField
      label="Category Data"
      v-model="catData"
      v-if="axisType == 'category'"
    />
    <div class="padding">
      <InputField label="Top" v-model="namePadding[0]" type="number" />
      <InputField label="Right" v-model="namePadding[1]" type="number" />
      <InputField label="Bottom" v-model="namePadding[2]" type="number" />
      <InputField label="Left" v-model="namePadding[3]" type="number" />
    </div>
    <Button text="Save" @click="saveXAxis" />
  </div>
</template>

<script setup>
import InputField from "./InputField.vue";
import Button from "./Button.vue";
import Switch from "./Switch.vue";
import SelectField from "./SelectField.vue";
import { ref } from "vue";

const props = defineProps({
  currentXAxis: Object,
});

const emit = defineEmits(["saveXAxis"]);
console.log(props.currentXAxis);
const showAxis = ref(
  props.currentXAxis != {} ? props.currentXAxis.show : false
);
const axisName = ref(props.currentXAxis != {} ? props.currentXAxis.name : "");
const axisType = ref(props.currentXAxis != {} ? props.currentXAxis.type : "");
const axisNameLocation = ref(
  props.currentXAxis != {} ? props.currentXAxis.nameLocation : ""
);
const catData = ref(
  props.currentXAxis != {}
    ? props.currentXAxis.data
      ? props.currentXAxis.data.join(" ")
      : ""
    : ""
);
const namePadding = ref(
  props.currentXAxis != {}
    ? props.currentXAxis.nameTextStyle
      ? props.currentXAxis.nameTextStyle.padding.map((pad) => pad.toString())
      : "0 0 0 0".split(" ")
    : "0 0 0 0".split(" ")
);

const saveXAxis = () => {
  let conf = {};

  if (showAxis.value) {
    conf = {
      ...conf,
      show: showAxis.value,
    };
  }

  if (axisName.value != "" && axisName.value) {
    conf = {
      ...conf,
      name: axisName.value,
    };
  }

  if (axisNameLocation.value != "" && axisNameLocation.value) {
    console.log("here nameloc", axisNameLocation.value);
    conf = {
      ...conf,
      nameLocation: axisNameLocation.value,
    };
  }

  if (axisType.value != "" && axisType.value) {
    conf = {
      ...conf,
      type: axisType.value,
    };
  }

  if (axisType.value == "category") {
    conf = {
      ...conf,
      data: catData.value.split(" "),
    };
  }

  if (namePadding.value != "" && namePadding.value) {
    conf = {
      ...conf,
      nameTextStyle: {
        padding: namePadding.value.map((pad) => parseInt(pad)),
      },
    };
  }

  console.log(conf);

  emit("saveXAxis", conf);
};
</script>

<style lang="scss" scoped>
.axis-config {
  height: 80%;
  width: 80%;

  background: #ececec;
  border-radius: 10px;

  padding: 15px 10%;

  display: flex;
  justify-content: center;
  align-items: center;
  flex-direction: column;

  box-shadow: 0px 0px 10px 0px rgba(139, 139, 139, 0.534);

  .buttons {
    display: flex;
    justify-content: flex-start;
    align-items: center;
    width: 80%;
  }

  .padding {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 80%;

    .input-field {
      width: 23%;
      background: #ececec;
    }
  }
}
</style>
