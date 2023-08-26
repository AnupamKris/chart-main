<template>
  <div class="series-config">
    <Button text="Create Series" @click="createSeries" />

    <div class="series-list">
      <div
        class="series"
        v-for="(series, index) in seriesList"
        :key="series.name"
        @click="setEditorSeries(index)"
      >
        <p>{{ series.name }}</p>
      </div>
    </div>
    <div class="series-editor" v-if="editorSeries != null">
      <InputField
        label="Series Name"
        id="series-name"
        v-model="seriesList[editorSeries].name"
      />
      <SelectField
        label="Series Type"
        id="series-type"
        :options="['bar', 'pie', 'line', 'scatter', 'boxplot']"
        v-model="seriesList[editorSeries].type"
      />
      <SelectField
        label="Color By"
        id="color-by"
        :options="['series', 'data']"
        v-model="seriesList[editorSeries].colorBy"
      />
      <TextArea
        label="Data"
        id="series-data"
        v-model="seriesList[editorSeries].data"
      />
    </div>
    <Button text="Save" @click="saveSeries" />
  </div>
</template>

<script setup>
import Button from "./Button.vue";
import InputField from "./InputField.vue";
import SelectField from "./SelectField.vue";

import { ref, onMounted } from "vue";
import TextArea from "./TextArea.vue";

const emit = defineEmits(["saveSeries"]);
const editorSeries = ref(null);
const props = defineProps({
  currentSeries: Array,
});
// change the series data to string
const seriesList = ref(
  props.currentSeries.map((series) => {
    return {
      ...series,
      data: JSON.stringify(series.data),
    };
  })
);

const setEditorSeries = (series) => {
  editorSeries.value = series;
};

const saveSeries = () => {
  console.log("saving series");
  // convert all the series data  to array
  let serl = seriesList.value.map((series) => {
    return {
      ...series,
      data: JSON.parse(series.data),
    };
  });
  emit("saveSeries", serl);
};

const createSeries = () => {
  console.log("SEE");
  seriesList.value.push({
    name: "series",
    type: "",
    data: "[]",
    colorBy: "series",
  });
};

onMounted(() => {
  console.log("series list", props.currentSeries);
});
</script>

<style lang="scss" scoped>
.series-config {
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

  .series-list {
    display: flex;
    // justify-content: center;
    align-items: center;
    height: 70px;
    width: 80%;
    background: #e2e2e2;
    border-radius: 10px;

    margin-top: 15px;

    .series {
      width: auto;
      height: calc(100% - 20px);
      background: #f3f3f3;
      border-radius: 10px;

      display: flex;
      justify-content: center;
      align-items: center;
      padding: 0 20px;
      margin-left: 10px;
    }
  }

  .series-editor {
    height: 400px;
    width: 80%;
    background: #e2e2e2;
    border-radius: 10px;

    padding: 0 20px;
    margin-top: 15px;
    padding-top: 15px;

    display: flex;
    // justify-content: center;
    align-items: center;
    flex-direction: column;

    .input-field {
      width: 100%;
      //   height: 50px;
      margin-top: 15px;
    }
  }
}
</style>
