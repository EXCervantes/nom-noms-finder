<template>
    <HeaderComponent />
    <h1>Hello User, Welcome to Add Resturant Page</h1>
    <form class="add-form">
        <input v-model="restaurant.name" type="text" name="name" placeholder="Enter Name">
        <input v-model="restaurant.address" type="text" name="address" placeholder="Enter Address">
        <input v-model="restaurant.contact" type="text" name="contact" placeholder="Enter Contact Number">
        <button type="button" class="add-form-bttn" @click="addRestaurant">
            Add New Restaurant
        </button>
    </form>
</template>
<script>
import HeaderComponent from './HeaderComponent.vue';
import axios from 'axios';
export default {
    name: 'AddResto',
    components: {
        HeaderComponent
    },
    data() {
        return {
            restaurant: {
                name: '',
                address: '',
                contact: '',
            }
        }
    },
    mounted() {
        let user = localStorage.getItem('user-info');
        if (!user) {
            this.$router.push({ name: 'SignUp' })
        }
    },
    methods: {
        async addRestaurant() {
            console.warn(this.restaurant)
            const result = await axios.post("http://localhost:3000/restaurant", {
                name: this.restaurant.name,
                address: this.restaurant.address,
                contact: this.restaurant.contact
            });
            if (result.status === 201) {
                this.$router.push({ name: 'HomePage' });
            }
            console.warn("result", result)
        }
    }
}
</script>