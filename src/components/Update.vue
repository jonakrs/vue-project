<template>
    <Header />
    <h1>Hello User, Welcome to the Update restaurant page</h1>
    <form class="add">
        <input type="text" name="name" placeholder="Enter Name" v-model="restaurant.name" />
        <input type="text" name="contact" placeholder="Enter contact" v-model="restaurant.contact" />
        <input type="text" name="address" placeholder="Enter address" v-model="restaurant.address" />
        <button type="button" v-on:click="updateRestaurant">Update Restaurant</button>
    </form>
</template>

<script>
import Header from './Header.vue';
import axios from 'axios';

export default {
    name: 'UpdatePage',
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
        async updateRestaurant() {
            try {
                console.warn(this.restaurant);
                const result = await axios.put("http://localhost:3000/restaurant/" + this.$route.params.id, {
                    name: this.restaurant.name,
                    contact: this.restaurant.contact,
                    address: this.restaurant.address
                });
                if (result.status == 200) {
                    this.$router.push({ name: 'Home' });
                }
            } catch (error) {
                console.error('Error updating restaurant:', error);
            }
        }
    },
    async mounted() {
        let user = localStorage.getItem('user-info');
        if (!user) {
            this.$router.push({ name: 'SignUp' });
            return; 
        }

        try {
            const result = await axios.get("http://localhost:3000/restaurant/" + this.$route.params.id);
         
            this.restaurant = result.data; 
            console.warn(result);
        } catch (error) {
            console.error('Error fetching restaurant data:', error);
        }
    }
}
</script>
