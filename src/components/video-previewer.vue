<template>
  <section class="video-previewer">
    <video :src="source" :poster="poster" controls muted class="video">
      抱歉，您的浏览器不支持内嵌视频
      <a :href="source">下载</a>
    </video>
  </section>
</template>

<script>
export default {
  name: "VideoPreviewer",
  props: {
    source: { type: String },
    poster: { type: String },
  },
  methods: {
    formatPoster(value) {
      return value || "";
    },
  },
};
</script>

<style lang="scss" scoped>
.video-previewer {
  .video {
    margin: 0 auto;
    display: block;
    width: 100%;
    height: 60vh;
  }
}
</style>