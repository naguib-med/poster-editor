<template>
  <div class="canvas-area">
    <canvas ref="canvas"></canvas>
  </div>
</template>

<script>
import { fabric } from "fabric";

export default {
  props: {
    width: {
      type: Number,
      required: true,
    },
    height: {
      type: Number,
      required: true,
    },
  },
  mounted() {
    this.canvas = new fabric.Canvas(this.$refs.canvas);

    // Définir les dimensions du canevas
    this.canvas.setWidth(this.width);
    this.canvas.setHeight(this.height);

    // Ajouter les écouteurs d'événements pour le bouton de suppression
    this.canvas.on('object:selected', this.onSelection);
    this.canvas.on('selection:cleared', this.onSelection);
  },
  methods: {
    onSelection(event) {
      this.$emit('selection-changed', !!event.target);
    }
  }
};
</script>

<style scoped>
.canvas-area {
    margin: 10px;
    border: 1px solid #ccc;
    height: 600px;
    width: 800px;
}
</style>
