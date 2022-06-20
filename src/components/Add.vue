<template>
    <Header />
    <h1>Hello {{ name }} Welcome to Add Restaurant Page</h1>

    <form class="add">
        <input type="text" v-model="restaurant.name" placeholder="Enter Name" />
        <input type="text" v-model="restaurant.contact" placeholder="Enter Contact" />
        <input type="text" v-model="restaurant.address" placeholder="Enter Address" />
        <button type="button" v-on:click="addRestaurant">Add New Restaurant</button>
    </form>
</template>

<script>
import axios from 'axios'
import Header from './Header.vue'
export default {
    name: 'Add',
    data() {
        return {
            name: '',
            restaurant: {
                name: '',
                contact: '',
                address: '',
            }
        }
    },
    methods: {
        async addRestaurant() {

            const result = await axios.post("http://localhost:3000/restaurants", {
                name: this.restaurant.name,
                contact: this.restaurant.contact,
                address: this.restaurant.address
            })
            console.warn("result", result)
            if (result.status == 201) {
                this.$router.push({ name: "Home" });
            }
        }
    },
    components: {
        Header
    },

    //Login Registering Lifecycle Hooks
    mounted() {
        let user = localStorage.getItem("user-info");
        this.name = JSON.parse(user).name
        if (!user) {
            this.$router.push({ name: "SignUp" })
        }
    }
}
</script>