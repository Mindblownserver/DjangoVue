<template>
    <div class="page-sign-up">
        <div class="columns">
            <div class="column is-4 is-offset-4">
                <h1 class="title">Log In</h1>
                <form @submit.prevent="submitForm">
                    <div class="field">
                        <label>Username:</label>
                        <div class="control">
                            <input type="text" class="input" v-model="username">
                        </div>
                    </div>
                    <div class="field">
                        <label>Password:</label>
                        <div class="control">
                            <input type="password" class="input" v-model="pass">
                        </div>
                    </div>
                    <div class="notification is-danger" v-if="errors.length">
                        <p v-for="error in errors" :key="error">{{ error }}</p>
                    </div>

                    <div class="field">
                        <div class="control">
                            <button class="button is-dark" >Log in</button>
                        </div>
                    </div>
                    <hr>
                    Or <router-link to="signup">Click here</router-link> to sign up!
                </form>
            </div>
        </div>
    </div>
</template>
<script>
import axios from "axios"
export default{
    name:"LogIn",
    data(){
        return{
            username:"",
            pass:"",
            errors:[]
        }
    },
    mounted(){
        document.title="Log in | Djackets"
    },
    methods:{
        async submitForm(){
            /* axios.defaults.headers.common["Authentication"]= ""
            localStorage.removeItem("token") */
            const formData={
                username:this.username,
                password:this.pass
            }
            await axios.post("/api/v1/token/login",formData).then(response=>{
                const token = response.data.auth_token
                console.log(token)
                this.$store.commit("setToken",token)
                
                axios.defaults.headers.common["Authorization"]= "Token "+token
                localStorage.setItem("token",token)
                
                console.log("Connected!")
               
                this.$router.push("/my-account")
            }).catch(error=>{
                if(error.response){
                    for(const property in error.response.data){
                        this.errors.push(`${property}: ${error.response.data[property]}`)
                    }
                    console.log(JSON.stringify(error.response.data))
                } else if(error.message){
                    this.errors.push("Something went wrong. Please try again")
                    console.log(JSON.stringify(error))
                }
            })
        
        }
    }
}
</script>