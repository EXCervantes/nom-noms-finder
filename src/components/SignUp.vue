<template>
    <img class="logo" src="../assets/resto-icon.png">
    <h1>Sign Up</h1>
    <div class="register">
        <input v-model="name" type="text" placeholder="Enter Name">
        <input v-model="email" type="text" placeholder="Enter Email">
        <input v-model="password" type="password" placeholder="Enter Password">
        <button class="signup-bttn" @click="signUp">
            Sign Up
        </button>
        <p>
            <router-link to="/login">
                Login
            </router-link>
        </p>
    </div>
</template>

<script>
import axios from 'axios'
export default {
    name: 'SignUp',
    data() {
        return {
            name: '',
            email: '',
            password: '',
        }
    },
    mounted() {
        let user = localStorage.getItem('user-info');
        if (user) {
            this.$router.push({ name: 'HomePage' })
        }
    },
    methods: {
        async signUp() {
            let result = await axios.post("http://localhost:3000/users", {
                name: this.name,
                email: this.email,
                password: this.password
            });

            console.warn(result);
            if (result.status === 201) {
                localStorage.setItem("user-info", JSON.stringify(result.data))
                this.$router.push({ name: 'HomePage' })
            }
        }
    }
}
</script>

<style></style>