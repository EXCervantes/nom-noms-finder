<template>
    <HeaderComponent />
    <h1>Hello User, Welcome to Update Restaurant Page.</h1>
    <form class="add-form">
        <input v-model="restaurant.name" type="text" name="name" placeholder="Enter Name">
        <input v-model="restaurant.address" type="text" name="address" placeholder="Enter Address">
        <input v-model="restaurant.contact" type="text" name="contact" placeholder="Enter Contact Number">
        <button type="button" class="add-form-bttn" @click="updateRestaurant">
            Update Restaurant
        </button>
    </form>
</template>
<script>
import HeaderComponent from './HeaderComponent.vue';
import axios from 'axios';
export default {
    name: 'UpdateResto',
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
    async mounted() {
        let user = localStorage.getItem('user-info');
        if (!user) {
            this.$router.push({ name: 'SignUp' })
        }
        const result = await axios.get('http://localhost:3000/restaurant/' + this.$route.params.id)
        console.log(result.data)
        this.restaurant = result.data
    },
    methods: {
        async updateRestaurant() {
            console.warn(this.restaurant)
            const result = await axios.put("http://localhost:3000/restaurant/" + this.$route.params.id, {
                name: this.restaurant.name,
                address: this.restaurant.address,
                contact: this.restaurant.contact
            });
            if (result.status === 200) {
                this.$router.push({ name: 'HomePage' });
            }
        }
    }
}
</script>