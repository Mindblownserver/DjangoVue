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
      <product-box v-for="product in latestProducts" :key="product.id" :product="product"/>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import axios from "axios"
import ProductBox from"@/components/ProductBox.vue"
export default {
  name: 'HomeView',
  data(){
    return{
      latestProducts:[]
    }
  },
  components: {
    ProductBox,
},
  mounted(){ // When the page has finished loading, we call this function "mounted"
    this.getLatestProducts()
  },
  methods:{
    async getLatestProducts(){
      this.$store.commit("setIsLoading", true)
      await axios.get("/api/v1/latest-products/").then(response=>{
        this.latestProducts=response.data
        document.title="Home | Djackets"
      }).catch(err=>{
        console.log(err)
      })
      this.$store.commit("setIsLoading", false)
    }
  }
}
</script>
