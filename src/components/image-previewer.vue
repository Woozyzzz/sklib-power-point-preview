<template>
  <section class="image-previewer">
    <img :src="source" :alt="alternative" class="image" />
  </section>
</template>

<script>
export default {
  name: "ImagePreviewer",
  props: {
    source: { type: String },
    alternative: { type: String },
  },
};
</script>

<style lang="scss" scoped>
.image-previewer {
  .image {
    margin: 0 auto;
    display: block;
    width: 100%;
  }
}
</style>