<template>
    <Header />
    <h1>Hello {{ name }} Welcome to Update Restaurant Page</h1>

    <form class="add">
        <input type="text" v-model="restaurant.name" placeholder="Enter Name" />
        <input type="text" v-model="restaurant.contact" placeholder="Enter Contact" />
        <input type="text" v-model="restaurant.address" placeholder="Enter Address" />
        <button type="button" v-on:click="updateRestaurant">Update Restaurant</button>
    </form>

</template>

<script>
import Header from './Header.vue'
import axios from 'axios'

export default {
    name: 'Update',
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
        async updateRestaurant() {

            console.warn(this.restaurant)
            const result = await axios.put('http://localhost:3000/restaurants/' + this.$route.params.id, {
                name: this.restaurant.name,
                contact: this.restaurant.contact,
                address: this.restaurant.address
            })
            if (result.status == 200) {
                this.$router.push({ name: "Home" });
            }
        }
    },
    components: {
        Header
    },

    //Login Registering Lifecycle Hooks
    async mounted() {
        let user = localStorage.getItem("user-info");
        this.name = JSON.parse(user).name
        if (!user) {
            this.$router.push({ name: "SignUp" })
        }
        const result = await axios.get('http://localhost:3000/restaurants/' + this.$route.params.id);
        // console.warn(this.$route.params.id)
        console.warn(result.data)
        this.restaurant = result.data
    }
}
</script>