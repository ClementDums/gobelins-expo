<template>
  <div class="o-page webgl-page">
    <section class="head">
      <h1 class="head__title">{{ $prismic.asText(title) }}</h1>
    </section>
    <section class="projets-list">
      <VueSlickCarousel v-bind="settings" ref="carousel">
          <FestivalProjectPreview
              v-for="(item, i) in projects"
              :key="i"
              :uid="item.uid"
              :data="item.data"
          />
      </VueSlickCarousel>
    </section>
    <!-- Slices block component -->
<!--    <slices-block :slices="slices"/>-->
  </div>
</template>

<script>
import VueSlickCarousel from 'vue-slick-carousel'
import 'vue-slick-carousel/dist/vue-slick-carousel.css'
// Imports for Prismic Slice components
import FestivalProjectPreview from '~/components/festival/FestivalProjectPreview'
import SlicesBlock from '~/components/SlicesBlock.vue'

export default {
  name: 'festival-page',
  components: {
    FestivalProjectPreview,
    SlicesBlock,
    VueSlickCarousel,
  },
  head () {
    return {
      title: 'Festival Annecy',
    }
  },
  async asyncData({ $prismic, params, error }) {
    try{
      const document = (await $prismic.api.getSingle('annecy-festival')).data;
      const projectsIds = document.projects.map(p => p.project.id);
      const projects = (await $prismic.api.getByIDs(projectsIds)).results;

      return {
        title: document.title,
        tab: document.body1,
        projects
      }
    } catch (e) {
      // Returns error page
      error({ statusCode: 404, message: 'Page not found' })
    }
  },
  data() {
    return {
      settings: {
        arrows: false,
        centerMode: true,
        centerPadding: '200px',
        focusOnSelect: true,
        infinite: true,
        slidesToShow: 1,
        speed: 500
      },
    }
  },
  mounted() {
    console.log(this.$refs.carousel);
  }
}
</script>

<style lang="scss">
  .head__title {
    margin: 100px auto;
    text-align: center;
  }
</style>
