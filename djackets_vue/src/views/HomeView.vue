<template>
  <div class="home">
    <section class="hero is-medium is-dark mb-6">
      <div class="hero-body has-text-centered">
        <p class="title mb-6">
          Welcome to Djacket
        </p>
        <p class="subtitle">
          The best jacket store out there
        </p>
      </div>
    </section>

    <div class="columns is-multiline">
      <div class="columns is-12"> 
        <h2 class="is-size-2 has-text-centered"> Latest Products</h2>
      </div>
      
      <div class="column is-3" v-for="product in latestProducts" :key="product.id">
        <div class="box">
          <figure class="image mb-4">
            <img :src="product.get_thumbnail">
          </figure>
          <h3 class="is-size-4">{{ product.name }}</h3>
          <p class="is-size-6 has-text-centered">{{ product.price }}</p>
          <router-link :to="product.get_absolute_url" class="button is-dark mt-4">View details</router-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios"
export default {
  name: 'HomeView',
  data(){
    return{
      latestProducts:[]
    }
  },
  components: {
  },
  mounted(){ // When the page has finished loading, we call this function "mounted"
    this.getLatestProducts()
  },
  methods:{
    async getLatestProducts(){
      this.$store.commit("setIsLoading", true)
      await axios.get("/api/v1/latest-products/").then(response=>{
        this.latestProducts=response.data
      }).catch(err=>{
        console.log(err)
      })
      this.$store.commit("setIsLoading", false)
    }
  }
}
</script>
<style scoped>
.image{
  margin-top: -1.25rem;
  margin-right: -1.25rem;
  margin-left: -1.25rem;
}
</style>