<template>
  <section class="audio-previewer">
    <audio :src="source" controls class="audio">
      抱歉，您的浏览器不支持内嵌音频
      <a :href="source">下载</a>
    </audio>
  </section>
</template>

<script>
export default {
  name: "AudioPreviewer",
  props: {
    source: { type: String },
  },
};
</script>

<style lang="scss" scoped>
.audio-previewer {
  .audio {
    margin: 0 auto;
    display: block;
    width: 100%;
  }
}
</style>