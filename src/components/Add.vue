<template>
    <Header />
    <h1>Hello User, Welcome to the Add Restaurant page</h1>
    <form class="add">
        <input type="text" name="name" placeholder="Enter Name" v-model="restaurant.name" />
        <input type="text" name="contact" placeholder="Enter contact" v-model="restaurant.contact" />
        <input type="text" name="address" placeholder="Enter address" v-model="restaurant.address" />
        <button type="button" v-on:click="addRestaurant">Add new Restaurant</button>

    </form>
</template>

<script>
import Header from './Header.vue';
import axios from 'axios';
export default {
    name: 'AddPage',
    components: {
        Header
    },
    data() {
        return {
            restaurant: {
                name: '',
                address: '',
                contact: ''
            }
        }

    },
    methods: {
       async addRestaurant() {
            console.warn(this.restaurant)
            const result = await axios.post("http://localhost:3000/restaurant",{
                name:this.restaurant.name,
                contact:this.restaurant.contact,
                address:this.restaurant.address
                

            });
            if(result.status==201)
            {
                this.$router.push({name:'Home'});
            }
            console.warn("result", result)
        }



    },
    mounted() {
        let user = localStorage.getItem('user-info');
        if (!user) {
            this.$router.push({ name: 'SignUp' });
        }
    }
}
</script>
