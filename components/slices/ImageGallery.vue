<template>
  <section class='image-gallery content-section'>
    <prismic-rich-text :field="slice.primary.name_of_the_gallery"/>
    <div class="gallery">
      <div v-for="item in slice.items" :key="item.id" class="gallery-item">
        <prismic-image :field="item.gallery_image"/>
        <prismic-rich-text :field="item.image_captions"/>
        <p>
          <!--<prismic-link :field="item.link" class="gallery-link">{{ $prismic.asText(item.link_label) }}</prismic-link>-->
        </p>
      </div>
    </div>
  </section>
</template>

<script>
export default {
  props: ['slice'],
  name: 'image-gallery'
}
</script>

<style lang="sass" scoped>
.gallery
  display: -webkit-box
  // OLD - iOS 6-, Safari 3.1-6, BB7
  display: -ms-flexbox
  // TWEENER - IE 10
  display: -webkit-flex
  // NEW - Safari 6.1+. iOS 7.1+, BB10
  display: flex
  -webkit-flex-wrap: wrap
  flex-wrap: wrap
  -webkit-justify-content: space-between
  justify-content: space-between

.gallery-item
  -webkit-box-flex: 0 1 48%
  -moz-box-flex: 0 1 48%
  -webkit-flex: 0 1 48%
  -ms-flex: 0 1 48%
  flex: 0 1 48%

.gallery-link
  margin-top: -20px
  text-transform: uppercase

</style>
