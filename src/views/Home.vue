<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
        <vueper-slides fade :touchable="false">
          <vueper-slide
            v-for="(slide, i) in slides"
            :key="i"
            :image="slide.image"/>
        </vueper-slides>
    </section>

    <div class="columns is-multiline">
      <div class="column is-12">
          <h2 class="is-size-2 has-text-centered">New Updates</h2>
      </div>

      <ProductBox 
        v-for="product in latestProducts"
        v-bind:key="product.id"
        v-bind:product="product" />
    </div>
  </div>
</template>

<script>
import axios from 'axios'
import ProductBox from '@/components/ProductBox'
import { VueperSlides, VueperSlide } from 'vueperslides'
import 'vueperslides/dist/vueperslides.css'

export default {
  name: 'Home',
  data() {
    return {
      slides: [
        {
          title: 'Banner1',
          content: 'banner event1',
          image: require('@/assets/banner1.jpg')
        },
        {
          title: 'Banner2',
          content: 'banner event2',
          image: require('@/assets/banner2.jpeg')
        },
        {
          title: 'Banner3',
          content: 'banner event3',
          image: require('@/assets/banner3.jpg')
        },
      ],
      latestProducts: [],
    }
  },
  components: {
    ProductBox,
    VueperSlides,
    VueperSlide,
  },
  mounted() {
    this.getLatestProducts()

    document.title = 'Djumper'
  },
  methods: {
    async getLatestProducts() {
      this.$store.commit('setIsLoading', true)

      await axios
        .get('/api/v1/latest-products/')
        .then(response => {
          this.latestProducts = response.data
        })
        .catch(error => {
          console.log(error)
        })

      this.$store.commit('setIsLoading', false)
    }
  }
}
</script>

<style lang="scss">
  .image {
    margin-top: -1.25rem;
    margin-left: -1.25rem;
    margin-right: -1.25rem;
  }
</style>
