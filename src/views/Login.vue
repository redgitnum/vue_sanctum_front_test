<template>
  <div class="flex flex-col justify-center items-center">
    <h1 class="text-2xl underline mb-4">Login page</h1>
    <div class="bg-white shadow-xl p-2 rounded">
      <form @submit.prevent="loginUser" class="flex flex-col gap-4 p-2 items-end">
        <label for="name">Email:
          <input type="email" name="email" ref="email" class="bg-blue-100 rounded p-2 ml-2 shadow-inner" v-model="user.email" required>
        </label>
        <label for="password">Password:
          <input type="password" name="password" class="bg-blue-100 rounded p-2 ml-2 shadow-inner" v-model="user.password" required>
        </label>
        <button type="submit" class="bg-green-500 p-2 shadow w-full text-white font-bold hover:bg-green-700 transition">Log in</button>
      </form>

    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data: function() {
    return{
      user: {
        email: '',
        password: '',
      },
      validationErrors: {
        email: '',
        password: '',
      }
    }
  },
  mounted: function() {
    this.initializeData();
  },
  methods: {
    initializeData: function() {
      this.user = {
        email: '',
        password: '',
      },
      this.validationErrors = {
        email: '',
        password: '',
      }
    },

    resetPasswords: function() {
      this.user.password = ''
    },

    loginUser: function() {
      axios
        .post('http://127.0.0.1:8000/api/login', this.user)
        .then(response=>response.data)
        .then((user) => {
          this.$emit('userdata', user)
        })
        .then(() => this.initializeData())
        .catch(e => {
          this.validationErrors = e.response.data.errors;
          this.resetPasswords();
          this.$refs.email.focus();
        })
    },

  }
}
</script>
