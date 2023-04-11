<template>
  <v-navigation-drawer app permanent width="350" class="sidebar" elevation="5">
    <v-list>
      <v-list-subheader class="text-h5">Draw</v-list-subheader>
      <v-list-item v-for="(draw, index) in brushTypes" :key="index" :value="draw.value" v-on:click="changeBrushType($event, draw.value)">
        <div class="text-icon-container">
          <div class="text">{{ draw.title }}</div>
          <v-icon class="icon">{{ draw.icon }}</v-icon>
        </div>
      </v-list-item>
    </v-list>

    <v-divider></v-divider>

    <v-row class="mt-5">
      <v-col cols="11" class="mx-auto">
        <v-color-picker v-model="brushColor" hide-inputs @update:modelValue="changeBrushColor"></v-color-picker>
      </v-col>
    </v-row>

    <v-row class="mt-5">
      <v-col cols="11" class="mx-auto">
        <v-slider v-model="brushWidth" step="1" min="1" max="50" thumb-label hide-details @input="changeBrushWidth"></v-slider>
      </v-col>
    </v-row>

    <v-row class="mt-5">
      <v-col cols="11" class="mx-auto">
        <v-btn color="primary" @click="startDrawing">Start drawing</v-btn>
      </v-col>
      <v-col cols="11" class="mx-auto">
        <v-btn color="success" @click="stopDrawing">Stop drawing</v-btn>
      </v-col>
      <v-col cols="11" class="mx-auto">
        <v-btn color="error" @click="clearCanvas">Clear canvas</v-btn>
      </v-col>
    </v-row>

  </v-navigation-drawer>
</template>

<script>
export default {
  data() {
    return {
      brushTypes: [
        { icon: "mdi-pencil", title: "Pencil", value: "pencil" },
        { icon: "mdi-square", title: "Square", value: "square" },
        { icon: "mdi-circle", title: "Circle", value: "circle" },
        { icon: "mdi-spray", title: "Spray", value: "spray" },
        { icon: "mdi-grid", title: "Pattern", value: "pattern" },
      ],
      brushColor: "#000000",
      brushWidth: 5,
      currentBrush: "pencil",
    };
  },
  methods: {
    changeBrushType(e, value) {
      this.currentBrush = value;
      this.$emit("change-brush-type", this.currentBrush);
    },
    changeBrushColor() {
      this.$emit("change-brush-color", this.brushColor);
    },
    changeBrushWidth() {
      this.$emit("change-brush-width", this.brushWidth);
    },
    startDrawing() {
      this.$emit("start-drawing");
    },
    stopDrawing() {
      this.$emit("stop-drawing");
    },
    clearCanvas() {
      this.$emit("clear-canvas");
    }
  },
};
</script>

<style>
.text-icon-container {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
}
.text {
    font-size: 14px;
    font-weight: 500;
    margin-left: 10px;
}
.icon {
    font-size: 20px;
    margin-right: 10px;
}
</style>
