<template>
  <section class="audio-previewer">
    <audio :src="audioSource" controls class="audio">
      抱歉，您的浏览器不支持内嵌音频
      <a :href="audioSource">下载</a>
    </audio>
  </section>
</template>

<script>
export default {
  name: "AudioPreviewer",
  props: {
    audioSource: { type: String },
  },
};
</script>

<style lang="scss" scoped>
.audio-previewer {
  background-color: #ddd;

  .audio {
    width: 100%;
  }
}
</style>