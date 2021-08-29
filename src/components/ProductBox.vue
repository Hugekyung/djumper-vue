<template>
    <div class="column is-3">
        <div class="box">
            <figure class="image mb-4">
            <router-link v-bind:to="product.get_absolute_url">
              <img v-bind:src="product.get_thumbnail">
            </router-link>
            </figure>

            <h2 class="is-size-4"><strong>{{ product.name }}</strong></h2>
            <p class="is-size-6 has-text-grey">{{ product.price }}</p>

        </div>
    </div>
</template>

<script>
export default {
    name: 'ProductBox',
    props: {
        product: Object
    }
}
</script>

<style scoped>
  .image {
    margin-top: -1.25rem;
    margin-left: -1.25rem;
    margin-right: -1.25rem;
  }
</style>