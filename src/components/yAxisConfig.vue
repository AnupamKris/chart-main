<template>
  <div class="axis-config">
    <div class="buttons">
      <Switch label="Show Axis" v-model="showAxis" />
    </div>
    <InputField label="Axis Name" v-model="axisName" />
    <SelectField
      label="Axis Type"
      :options="['category', 'value', 'time', 'log']"
      id="y-axis-type"
      v-model="axisType"
    />
    <SelectField
      label="Axis Name Location"
      :options="['start', 'center', 'end']"
      id="y-axis-name-location"
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
    <Button text="Save" @click="saveYAxis" />
  </div>
</template>

<script setup>
import InputField from "./InputField.vue";
import Button from "./Button.vue";
import Switch from "./Switch.vue";
import SelectField from "./SelectField.vue";
import { ref } from "vue";

const props = defineProps({
  currentYAxis: Object,
});

const emit = defineEmits(["saveYAxis"]);
console.log(props.currentYAxis);
const showAxis = ref(
  props.currentYAxis != {} ? props.currentYAxis.show : false
);
const axisName = ref(props.currentYAxis != {} ? props.currentYAxis.name : "");
const axisType = ref(props.currentYAxis != {} ? props.currentYAxis.type : "");
const axisNameLocation = ref(
  props.currentYAxis != {} ? props.currentYAxis.nameLocation : ""
);
const catData = ref(
  props.currentYAxis != {}
    ? props.currentYAxis.data
      ? props.currentYAxis.data.join(" ")
      : ""
    : ""
);
const namePadding = ref(
  props.currentYAxis != {}
    ? props.currentYAxis.nameTextStyle
      ? props.currentYAxis.nameTextStyle.padding.map((pad) => pad.toString())
      : "0 0 0 0".split(" ")
    : "0 0 0 0".split(" ")
);

const saveYAxis = () => {
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

  emit("saveYAxis", conf);
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
