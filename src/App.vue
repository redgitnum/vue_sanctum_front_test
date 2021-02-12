<template>
  <div id="app" class="bg-gray-100 min-h-screen">
    <div id="nav" class="w-full bg-gray-300 p-1 mb-4 flex justify-end pr-4 gap-4 text-xl border-b border-gray-400 text-gray-700">
      <div v-if="logged" class="w-full ml-4">Logged as {{ user.name }}</div>
      <router-link to="/" class="hover:text-gray-900 self-end transition border-r border-gray-400 pr-4">Login</router-link>
      <router-link to="/register" class="hover:text-gray-900 transition">Register</router-link>
      <!-- <button @click="logout" class="hover:text-gray-900 transition border-l border-gray-400 pl-4">Logout</button> -->
      <button @click="isAuth" class="p-2 bg-gray-300">Protected route</button>
    </div>
    <router-view @userdata ='getUser'/>
  </div>
</template>
<script lang="ts">
import axios from 'axios';
import Vue from 'vue'
export default Vue.extend({
  data: function() {
    return{
      user: ''
    }
  },
  computed: {
    logged: function(){
      return this.user !== '';
    }
  },
  methods: {
    getUser: function(user){
      this.user = user
    },
    isAuth: function(){
      axios
        .get('http://127.0.0.1:8000/api/protected', {headers: { Authorization: `Bearer ${this.user.authToken}` }})
        .then(response => response.data)
        .then(console.log)
    }
  }
  
})
</script>

<style>
</style>
