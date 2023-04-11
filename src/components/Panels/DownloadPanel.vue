<template>
  <v-navigation-drawer app width="350" elevation="5" permanent>
    <v-list>
      <v-list-subheader class="text-h5">Load</v-list-subheader>
      <v-list-item v-for="(type, index) in loadType" :key="index" :value="type.value" v-on:click="load($event, type.value)">
        <div class="text-icon-container">
          <div class="text">{{ type.title }}</div>
          <v-icon class="icon">{{ type.icon }}</v-icon>
        </div>
      </v-list-item>
    </v-list>
    <v-list>
      <v-list-subheader class="text-h5">Download</v-list-subheader>
      <v-list-item v-for="(type, index) in downloadType" :key="index" :value="type.value" v-on:click="download($event, type.value)">
        <div class="text-icon-container">
          <div class="text">{{ type.title }}</div>
          <v-icon class="icon">{{ type.icon }}</v-icon>
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
      loadType: [
        { icon: "mdi mdi-svg", title: "Load SVG", value: "svg" },
        { icon: "mdi mdi-code-json", title: "Load JSON", value: "json" },
      ],
      downloadType: [
        { icon: "mdi-file-image", title: "Download Image", value: "image" },
        { icon: "mdi mdi-svg", title: "Download SVG", value: "svg" },
        { icon: "mdi mdi-code-json", title: "Download JSON", value: "json" },
      ],
    };
  },

  methods: {
    load(e, value) {
      this.$emit("load", e, value);
    },
    download(e, value) {
      this.$emit("download", e, value);
    },
  },
};

</script>
