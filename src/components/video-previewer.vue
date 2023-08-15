<template>
  <section class="video-previewer">
    <video :src="videoSource" controls muted class="video">
      抱歉，您的浏览器不支持内嵌视频
      <a :href="videoSource">下载</a>
    </video>
  </section>
</template>

<script>
export default {
  name: "VideoPreviewer",
  props: {
    videoSource: { type: String },
  },
};
</script>

<style lang="scss" scoped>
.video-previewer {
  background-color: #000;

  .video {
    width: 100%;
  }
}
</style>