<template>
    <HeaderComponent />
    <h1>Hello {{ name }}, Welcome to the app!</h1>
    <table>
        <caption>Local restaurants around the area</caption>
        <thead>
            <tr>
                <th scope="col">
                    ID
                </th>
                <th scope="col">
                    Name
                </th>
                <th scope="col">
                    Address
                </th>
                <th scope="col">
                    Contact
                </th>
                <th scope="col">
                    Actions
                </th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in restaurant" :key="item.id">
                <td>{{ item.id }}</td>
                <td>{{ item.name }}</td>
                <td>{{ item.address }}</td>
                <td>{{ item.contact }}</td>
                <td>
                    <button id="update-bttn">
                        <router-link id="update-link" :to="'/update-restaurant/' + item.id">
                            Update
                        </router-link>
                    </button>
                    <button id="delete-bttn" @click="deleteRestaurant(item.id)">
                        Delete
                    </button>
                </td>
            </tr>
        </tbody>
    </table>
</template>
<script>
import HeaderComponent from './HeaderComponent.vue';
import axios from 'axios';
export default {
    name: 'HomePage',

    components: {
        HeaderComponent
    },
    data() {
        return {
            name: '',
            restaurant: []
        }
    },
    async mounted() {
        this.loadData();
    },
    methods: {
        async deleteRestaurant(id) {
            let result = await axios.delete('http://localhost:3000/restaurant/' + id);
            console.warn(result)
            if (result.status === 200) {
                this.loadData()
            }
        },
        async loadData() {
            let user = localStorage.getItem('user-info');
            this.name = JSON.parse(user).name;
            if (!user) {
                this.$router.push({ name: 'SignUp' })
            }
            let result = await axios.get('http://localhost:3000/restaurant');
            this.restaurant = result.data;
        }
    }
}
</script>
<style>
table {
    border-collapse: collapse;
    border: 2px solid rgb(140 140 140);
    font-family: sans-serif;
    font-size: 1rem;
    letter-spacing: 1px;
    margin-left: 20px;
}

caption {
    caption-side: bottom;
    padding: 10px;
    font-weight: bold;
}

thead,
tfoot {
    background-color: rgb(228 240 245);
}

th,
td {
    border: 1px solid rgb(160 160 160);
    padding: 8px 10px;
    width: 190px;
    height: 40px;
}

td:last-of-type {
    text-align: center;
}

tbody>tr:nth-of-type(even) {
    background-color: rgb(237 238 242);
}

tfoot th {
    text-align: right;
}

tfoot td {
    font-weight: bold;
}

#update-bttn {
    margin-right: 4px;
}

#update-link {
    text-decoration: none;
}

#delete-bttn {
    margin-left: 4px;
}
</style>