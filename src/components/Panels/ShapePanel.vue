<template>
  <v-navigation-drawer app permanent width="350" class="sidebar" elevation="5">
    <v-list>
      <v-list-subheader class="text-h5">Shape button</v-list-subheader>
      <v-list-item v-for="(shape, index) in shapes" :key="index" :value="shape.value" v-on:click="addShape($event, shape.value)">
        <div class="text-icon-container">
          <div class="text">{{ shape.title }}</div>
          <v-icon class="icon">{{ shape.icon }}</v-icon>
        </div>
      </v-list-item>
    </v-list>

    <v-divider></v-divider>

    <v-row class="mt-5">
      <v-col cols="11" class="mx-auto">
        <v-color-picker v-model="shapeColor" hide-inputs  @update:modelValue="changeShapeColor"></v-color-picker>
      </v-col>
    </v-row>
  </v-navigation-drawer>
</template>


<script>
export default {
  components: {
  },
  data() {
    return {
      shapes: [
        { icon: "mdi-square", title: "Square", value: "square" },
        { icon: "mdi-rectangle", title: "Rectangle", value: "rectangle" },
        { icon: "mdi-circle", title: "Circle", value: "circle" },
        { icon: "mdi-triangle", title: "Triangle", value: "triangle" },
        { icon: "mdi-ellipse", title: "Ellipse", value: "ellipse" },
      ],
      shapeColor: "#000000",
    };
  },

  methods: {
    addShape(e, value) {
      this.$emit("add-shape", e, value);
    },
    changeShapeColor() {
      this.$emit("change-shape-color", this.shapeColor);
    },
  },
};
</script>

<style >
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
