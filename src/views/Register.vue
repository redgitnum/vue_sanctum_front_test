<template>
  <div class="flex flex-col justify-center items-center">
    <h1 class="text-2xl underline mb-4">Register page</h1>
    <div class="bg-white shadow-xl p-2 rounded">
      <form @submit.prevent="registerUser" class="flex flex-col gap-4 p-2 items-end">
        <div class="text-red-500 text-xs uppercase" :v-if="validationErrors.name">{{ validationErrors.name[0] }}</div>
        <label for="name">Name:
          <input type="text" name="name" ref="name" class="bg-blue-100 rounded p-2 ml-2 shadow-inner" v-model="user.name" required>
        </label>
        <label for="name">Email:
          <input type="email" name="email" class="bg-blue-100 rounded p-2 ml-2 shadow-inner" v-model="user.email" required>
        </label>
        <label for="password">Password:
          <input type="password" name="password" class="bg-blue-100 rounded p-2 ml-2 shadow-inner" v-model="user.password" required>
        </label>
        <label for="confirm-password">Confirm password:
          <input type="password" name="confirm-password" class="bg-blue-100 rounded p-2 ml-2 shadow-inner" v-model="user.confirmPassword" required>
        </label>
        <button type="submit" class="bg-green-500 p-2 shadow w-full text-white font-bold hover:bg-green-700 transition">Create new User</button>
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
        name: '',
        email: '',
        password: '',
        confirmPassword: ''
      },
      validationErrors: {
        name: '',
        email: '',
        password: '',
      }
    }
  },
  computed: {
    passwordMatch: function() {
      return this.user.password === this.user.confirmPassword
    }
  },
  mounted: function() {
    this.initializeData();
  },
  methods: {
    initializeData: function() {
      this.user = {
        name: '',
        email: '',
        password: '',
        confirmPassword: ''
      },
      this.validationErrors = {
        name: '',
        email: '',
        password: '',
      }
    },

    resetPasswords: function() {
      this.user.password = ''
      this.user.confirmPassword = ''
    },

    registerUser: function() {
      if(this.passwordMatch){
        axios
          .post('http://127.0.0.1:8000/api/register', this.user)
          .then(response=>response.data)
          .then(console.log)
          .then(() => this.initializeData())
          .catch(e => {
            this.validationErrors = e.response.data.errors;
            this.resetPasswords();
            this.$refs.name.focus();
          })
      }
    },

  }
}
</script>
