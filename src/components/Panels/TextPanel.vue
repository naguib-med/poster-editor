<template>
  <v-navigation-drawer app permanent width="350" class="sidebar" elevation="5">
    <v-list>
      <v-list-subheader class="text-h5">Texts</v-list-subheader>
      <v-list-item v-for="(type, index) in textType" :key="index" :value="type.value" v-on:click="addText($event, type.value)">
        <div class="text-icon-container">
          <div class="text">{{ type.title }}</div>
          <v-icon class="icon">{{ type.icon }}</v-icon>
        </div>
      </v-list-item>
    </v-list>
    <v-divider></v-divider>
      <v-list>
      <v-list-subheader class="text-h5">Tools</v-list-subheader>
      <v-list-item v-for="(tool, index) in textTools" :key="index" :value="tool.value"  v-on:click="applyTextTool($event, tool)">
        <div class="text-icon-container">
          <div class="text">{{ tool.title }}</div>
          <v-icon class="icon">{{ tool.icon }}</v-icon>
        </div>
      </v-list-item>
    </v-list>
  </v-navigation-drawer>
</template>

<script>
export default {
  props: {
    canvas: {
      type: Object,
      default: null
    }
  },
  components: {
  },
  data() {
    return {
      textType: [
        { title: "Add Title", value: "title" },
        { title: "Add Subtitle", value: "subtitle" },
        { title: "Add Paragraph", value: "paragraph" },
      ],
      textTools: [
        { icon: "mdi-format-color-text", title: "Font Color", value: "color" },
        { icon: "mdi-format-size", title: "Font Size", value: "size" },
        { icon: "mdi-format-bold", title: "Bold", value: "bold" },
        { icon: "mdi-format-italic", title: "Italic", value: "italic" },
        { icon: "mdi-format-underline", title: "Underline", value: "underline" },
        { icon:"mdi-format-text", title: "Stroke Through Text", value: "linethrough"},
      ],
    };
  },

  methods: {
    addText(e, value) {
      this.$emit("add-text", e, value);
    },
    applyTextTool(e, tool) {
      this.$emit("apply-text-tool", e, tool);
    },
  },
};
</script>

<style scoped>
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

.sidebar {
    background-color: #f5f5f5;
    box-shadow: none;
}

.subheader {
    font-size: 18px;
    font-weight: 500;
    color: #333333;
    padding: 16px 0;
}

.icon {
    font-size: 20px;
    color: #999999;
    margin-left: 12px;
}

</style>
