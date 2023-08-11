<template>
    <div class="page-categroy">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h2 class="is-size has-text-centered">{{ category.name }}</h2>
            </div>
            <product-box v-for="product in category.products" :key="product.id" :product="product"/>
        </div>
    </div>
</template>
<script>
import axios from "axios"
import {toast} from "bulma-toast"
import ProductBox from"@/components/ProductBox.vue"

export default {
    name:"Category",
    data(){
        return{
            category:{
                products:[]
            }
        }
    },
    components: {
    ProductBox,
    },
    mounted(){
        this.getCategroy()
    },
    watch:{
        $route(to,from){
            if(to.name==="Category"){ // Category is this view's name
                this.getCategroy()
            }
        }
    },
    methods:{
        async getCategroy(){
            const category_slug= this.$route.params.category_slug
            this.$store.commit("setIsLoading",true)
            await axios.get(`api/v1/products/${category_slug}`).then(response=>{
                this.category=response.data
                document.title=this.category.name+' | Djackets'
            }).catch(err=>{
                console.log(err)
                toast({
                    message:"Something went wrong!",
                    type:"is-danger",
                    dismissible:true,
                    pauseOnHover:true,
                    duration:2000,
                    position:"bottom-right"
                })
            })
            this.$store.commit("setIsLoading",false)
        }
    }
}
</script>