<template>
  <article id="app">
    <template v-for="(item, index) in slideSchemaList">
      <ImagePreviewerVue
        :key="index"
        :imageSource="formatImagePreviewerImageSource(item.name)"
        :imageAlternative="item.alt"
      />
    </template>
  </article>
</template>

<script>
import ImagePreviewerVue from "@/components/image-previewer.vue";

export default {
  name: "App",
  components: { ImagePreviewerVue },
  data() {
    return {
      slideSchemaList: [],
    };
  },
  methods: {
    async fetchSlideSchemaList() {
      return await fetch("/slide-schema-list.json").then((response) =>
        response.json()
      );
    },

    async handleFetchSlideSchemaList() {
      const response = await this.fetchSlideSchemaList().catch((error) => {
        throw error;
      });
      this.slideSchemaList = Array.isArray(response) ? response : [];
    },

    formatImagePreviewerImageSource(name) {
      return name ? `/images/${name}` : "";
    },
  },
  async created() {
    await this.handleFetchSlideSchemaList().catch((error) => {
      throw error;
    });
  },
};
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
</style>
