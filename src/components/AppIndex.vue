<template>
  <div class="login">
    <input type="text" name="" placeholder="email" v-model="email" :disabled="disabled">
    <input type="password" name="" placeholder="password" v-model="password" :disabled="disabled">
    <button @click="login()" :disabled="disabled">Login</button>
    <button @click="register()" class="register-btn">Register</button>
  </div>
</template>

<script>
import Vue from 'vue'
import {firebaseApp} from '../firebase'
import VueFire from 'vuefire'

Vue.use(VueFire)
export default {
  name: 'AppIndex',
  methods: {
    login: function () {
      var vm = this
      this.toggleDisabled()
      firebaseApp.auth().signInWithEmailAndPassword(this.email, this.password)
      .then(function (data) {
        // Handle successful login
        vm.$router.push({name: 'TodosIndex'})
      }).catch(function (error) {
        // Failed to login, some error occurred.
        if (error.code === 'auth/user-not-found') {
          var confirmed = confirm(error + ',register instead?')
          if (confirmed) {
            firebaseApp.auth().createUserWithEmailAndPassword(vm.email, vm.password)
            alert('Registered, Try again to log in')
            vm.email = ''
            vm.password = ''
          }
        } else {
          alert(error)
        }
        console.log(error)
      })
    },
    register: function () {
      firebaseApp.auth().createUserWithEmailAndPassword(this.email, this.password)
        .catch(function (error) {
          // Handle Errors here.
          var errorCode = error.code
          var errorMessage = error.message
          if (errorCode === 'auth/weak-password') {
            alert('The password is too weak.')
          } else {
            alert(errorMessage)
          }
          console.log(error)
        })
    },
    toggleDisabled: function () {
      this.disabled = !this.disabled
    }
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      email: null,
      password: null,
      disabled: false,
      auth: {}
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.login {
  width: 70%;
  margin-top: 50px;
  margin-bottom: 50px;
  margin-left: 15%;  
}

.register-btn {
  background: #fff;
  color: #004D40;
  border: 1px solid #4DB6AC;
}
</style>
