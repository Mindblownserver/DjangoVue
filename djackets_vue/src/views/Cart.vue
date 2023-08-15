<template>
    <div class="page-cart">
        <div class="columns is-multiline">
            <div class="column is-12">
                <h1 class="title">Cart</h1>
            </div>
            <div class="column is-12 box">
                <table class="table is-fullwidth" v-if="cartTotalLength">
                    <thead>
                        <tr>
                            <th>Product</th>
                            <th>Price</th>
                            <th>Quantity</th>
                            <th>Total</th>
                            <th></th>
                        </tr>
                    </thead>
                    <tbody>
                        <CardItem v-for="item in cart.items" :key="item.product.id" :initialItem="item" v-on:removeFromCart="removeFromCart"/>
                    </tbody>
                </table>
                <p v-else>You don't have any products in your cart...</p>
            </div>
            <div class="column is-12 box">
                <h2 class="subtitle">Summary</h2>
                <strong>{{ cartTotalPrice.toFixed(3) }} TND</strong>, {{ cartTotalLength }} items
                <hr>
                <router-link to="/cart/checkout" class="button is-dark">Proceed to checkout</router-link>
            </div>
        </div>
    </div>
</template>
<script>
import CardItem from "@/components/CardItem.vue";
export default{
    name:"Cart",
    components:{
        CardItem
    },
    data(){
        return{
            cart:{
                items:[]
            }
        }
    },
    mounted(){
        this.cart=this.$store.state.cart
    },
    computed:{
        cartTotalLength(){
            return this.cart.items.reduce((acc,cruVal)=>{
                return acc+=cruVal.quantity
            },0)
        },
        cartTotalPrice(){
            return this.cart.items.reduce((acc,cruVal)=>{
                return acc+=cruVal.quantity * cruVal.product.price
            },0)
        }
    },
    methods:{
        removeFromCart(item){
            this.cart.items=this.cart.items.filter(i=>i.product.id!==item.product.id) // remove items that have the same id as the variable item passed as parameter 
        }
    }
}
</script>