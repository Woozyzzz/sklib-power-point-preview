<template>
  <article id="app">
    <template
      v-for="(
        {
          type,
          local,
          source,
          alternative,
          posterLocal,
          poster,
          content,
          href,
        },
        index
      ) in slideSchemaList"
    >
      <image-previewer-vue
        v-if="type === 'image'"
        :key="index"
        :source="formatPreviewerSource(type, local, source)"
        :alternative="alternative"
      />
      <video-previewer-vue
        v-else-if="type === 'video'"
        :key="index"
        :source="formatPreviewerSource(type, local, source)"
        :poster="formatVideoPreviewerPoster(posterLocal, poster)"
      />
      <audio-previewer-vue
        v-else-if="type === 'audio'"
        :key="index"
        :source="formatPreviewerSource(type, local, source)"
      />
      <pre
        v-else-if="type === 'text'"
        v-text="formatTextContent(content)"
        :key="index"
        class="pre"
      ></pre>
      <p v-else-if="type === 'anchor'" :key="index" class="paragraph">
        <a
          v-text="formatTextContent(content)"
          :href="href"
          target="_blank"
          class="anchor"
        ></a>
      </p>
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
      return await fetch(`${process.env.BASE_URL}slide-schema-list.json`).then(
        (response) => response.json()
      );
    },

    async handleFetchSlideSchemaList() {
      const response = await this.fetchSlideSchemaList().catch((error) => {
        throw error;
      });
      this.slideSchemaList = Array.isArray(response) ? response : [];
    },

    formatPreviewerSource(type, local, source) {
      if (!local) {
        return source || "";
      }
      const sourcePrefixHashMap = new Map()
        .set("image", `${process.env.BASE_URL}images/`)
        .set("video", `${process.env.BASE_URL}videos/`)
        .set("audio", `${process.env.BASE_URL}audios/`);
      const sourcePrefix = sourcePrefixHashMap.get(type) || "";
      return sourcePrefix ? (source ? `${sourcePrefix}${source}` : "") : "";
    },
    formatVideoPreviewerPoster(posterLocal, poster) {
      if (!posterLocal) {
        return poster || "";
      }
      const posterPrefix = `${process.env.BASE_URL}images/`;
      return poster ? `${posterPrefix}${poster}` : "";
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
    font-size: 16px;
    white-space: pre-wrap;
  }

  .paragraph {
    padding: 8px 16px;

    .anchor {
      color: #409eff;
      border-bottom: 1px solid #409eff;
    }
  }
}
</style>
