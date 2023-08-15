<template>
  <section class="image-previewer">
    <img :src="imageSource" :alt="imageAlternative" class="image" />
  </section>
</template>

<script>
export default {
  name: "ImagePreviewer",
  props: {
    imageSource: { type: String },
    imageAlternative: { type: String },
  },
};
</script>

<style lang="scss" scoped>
.image-previewer {
  .image {
    width: 100%;
    background-color: #eee;
  }
}
</style>