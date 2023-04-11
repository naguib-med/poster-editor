<template>
  <v-navigation-drawer app permanent width="350" class="sidebar" elevation="5">
    <h5 class="text-h5">Brush</h5>
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
