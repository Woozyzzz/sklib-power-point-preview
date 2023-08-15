<template>
  <article id="app">
    <template v-for="(item, index) in slideSchemaList">
      <image-previewer-vue
        v-if="item.type === 'image'"
        :key="index"
        :imageSource="formatPreviewerSource(item.type, item.name)"
        :imageAlternative="item.alt"
      />
      <video-previewer-vue
        v-else-if="item.type === 'video'"
        :key="index"
        :videoSource="formatPreviewerSource(item.type, item.name)"
      />
      <audio-previewer-vue
        v-else-if="item.type === 'audio'"
        :key="index"
        :audioSource="formatPreviewerSource(item.type, item.name)"
      />
      <pre v-else-if="item.type === 'text'" :key="index" class="pre">{{
        formatTextContent(item.content)
      }}</pre>
    </template>
  </article>
</template>

<script>
import ImagePreviewerVue from "@/components/image-previewer.vue";
import VideoPreviewerVue from "@/components/video-previewer.vue";
import AudioPreviewerVue from "@/components/audio-previewer.vue";

export default {
  name: "App",
  components: { ImagePreviewerVue, VideoPreviewerVue, AudioPreviewerVue },
  data() {
    return {
      slideSchemaList: [],
    };
  },
  methods: {
    async fetchSlideSchemaList() {
      return await fetch(
        `${
          process.env.NODE_ENV === "production"
            ? "/sklib-power-point-preview/"
            : "/"
        }slide-schema-list.json`
      ).then((response) => response.json());
    },

    async handleFetchSlideSchemaList() {
      const response = await this.fetchSlideSchemaList().catch((error) => {
        throw error;
      });
      this.slideSchemaList = Array.isArray(response) ? response : [];
    },

    formatPreviewerSource(type, name) {
      const sourcePrefixHashMap = new Map()
        .set("image", "/images/")
        .set("video", "/videos/")
        .set("audio", "/audios/");
      const sourcePrefix = sourcePrefixHashMap.get(type) || "";
      return sourcePrefix ? (name ? `${sourcePrefix}${name}` : "") : "";
    },
    formatTextContent(value) {
      return value || "";
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
  color: #2c3e50;

  .pre {
    padding: 8px 16px;
    white-space: pre-wrap;
  }
}
</style>
