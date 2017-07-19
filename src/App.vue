<template>
  <div id="app">
    <div class="title">
      <div class="nav-button" @click="logOut()" v-if="$route.path !== '/'">
        <i class="material-icons">arrow_back</i>
      </div>
      <h1>Simple Todo App</h1>
    </div>
    <div class="content">
      <router-view></router-view>
    </div>
  </div>
</template>

<script>
import {firebaseApp} from './firebase'

export default {
  name: 'app',
  computed: {
    modal: function () {
      return this.showModal
    }
  },
  methods: {
    backBtn: function () {
      this.$router.go(-1)
    },
    logOut () {
      firebaseApp.auth().signOut()
    }
  },
  data () {
    return {
      showModal: true
    }
  }
}
</script>

<style>
body, html {
  height: 100%;
  margin: 3% 0 0 0;
  font-family: 'Open Sans', sans-serif;
  background: #e2e1e0;
  font-size: 30px;
}

.row {    
  display: block;
  clear:both;
}
/* For mobile phones: */
[class*="col-"] {
  width: 100%;
}
@media only screen and (min-width: 600px) {
  /* For tablets: */
  [class*="col-"] {float: left;}
  .col-x-1 {width: 8.33%;}
  .col-x-2 {width: 16.66%;}
  .col-x-3 {width: 25%;}
  .col-x-4 {width: 33.33%;}
  .col-x-5 {width: 41.66%;}
  .col-x-6 {width: 50%;}
  .col-x-7 {width: 58.33%;}
  .col-x-8 {width: 66.66%;}
  .col-x-9 {width: 75%;}
  .col-x-10 {width: 83.33%;}
  .col-x-11 {width: 91.66%;}
  .col-x-12 {width: 100%;}
  .col-offset-x-1 {margin-left: 8.33%;}
  .col-offset-x-2 {margin-left: 16.66%;}
  .col-offset-x-3 {margin-left: 25%;}
  .col-offset-x-4 {margin-left: 33.33%;}
  .col-offset-x-5 {margin-left: 41.66%;}
  .col-offset-x-6 {margin-left: 50%;}
  .col-offset-x-7 {margin-left: 58.33%;}
  .col-offset-x-8 {margin-left: 66.66%;}
  .col-offset-x-9 {margin-left: 75%;}
  .col-offset-x-10 {margin-left: 83.33%;}
  .col-offset-x-11 {margin-left: 91.66%;}
  .col-offset-x-12 {margin-left: 100%;}
}
@media only screen and (min-width: 768px) {
  /* For mini desktop: */
  [class*="col-"] {float: left;}
  .col-m-1 {width: 8.33%;}
  .col-m-2 {width: 16.66%;}
  .col-m-3 {width: 25%;}
  .col-m-4 {width: 33.33%;}
  .col-m-5 {width: 41.66%;}
  .col-m-6 {width: 50%;}
  .col-m-7 {width: 58.33%;}
  .col-m-8 {width: 66.66%;}
  .col-m-9 {width: 75%;}
  .col-m-10 {width: 83.33%;}
  .col-m-11 {width: 91.66%;}
  .col-m-12 {width: 100%;}
  .col-offset-m-1 {margin-left: 8.33%;}
  .col-offset-m-2 {margin-left: 16.66%;}
  .col-offset-m-3 {margin-left: 25%;}
  .col-offset-m-4 {margin-left: 33.33%;}
  .col-offset-m-5 {margin-left: 41.66%;}
  .col-offset-m-6 {margin-left: 50%;}
  .col-offset-m-7 {margin-left: 58.33%;}
  .col-offset-m-8 {margin-left: 66.66%;}
  .col-offset-m-9 {margin-left: 75%;}
  .col-offset-m-10 {margin-left: 83.33%;}
  .col-offset-m-11 {margin-left: 91.66%;}
  .col-offset-m-12 {margin-left: 100%;}
}
@media only screen and (min-width: 980px) {
  /* For desktop: */
  [class*="col-"] {float: left;}
  .col-1 {width: 8.33%;}
  .col-2 {width: 16.66%;}
  .col-3 {width: 25%;}
  .col-4 {width: 33.33%;}
  .col-5 {width: 41.66%;}
  .col-6 {width: 50%;}
  .col-7 {width: 58.33%;}
  .col-8 {width: 66.66%;}
  .col-9 {width: 75%;}
  .col-10 {width: 83.33%;}
  .col-11 {width: 91.66%;}
  .col-12 {width: 100%;}
  .col-offset-1 {margin-left: 8.33%;}
  .col-offset-2 {margin-left: 16.66%;}
  .col-offset-3 {margin-left: 25%;}
  .col-offset-4 {margin-left: 33.33%;}
  .col-offset-5 {margin-left: 41.66%;}
  .col-offset-6 {margin-left: 50%;}
  .col-offset-7 {margin-left: 58.33%;}
  .col-offset-8 {margin-left: 66.66%;}
  .col-offset-9 {margin-left: 75%;}
  .col-offset-10 {margin-left: 83.33%;}
  .col-offset-11 {margin-left: 91.66%;}
  .col-offset-12 {margin-left: 100%;}
}

#app {
  position: relative;
  width: 40%;
  margin-left: 30%;
  background-color: #fff;
  border: 1px solid #4DB6AC;
  padding-bottom: 10px;
  box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);  
}

.title {
  position: relative;
  padding: 10px;
  height: 100px;  
  color: #fff;
  background-color: #4DB6AC;  
}

.nav-button {
  position: absolute;
  top: 5px;
  left: 5px;
  cursor: pointer;
}
.nav-button i {
  font-size: 30px;
}

.content {
  width: 100%;
}

h1 {
  font-weight: normal;
  font-size: 50px;
  margin-bottom: 5px;
}

input {  
  box-sizing: border-box;
  padding-left: 5px;
  padding-right: 5px;
  /*width: 100%;*/
  height: 40px;
  margin-bottom: 10px;
  font-size: 30px;
}

button {
  width: 100%;
  height: 40px;
  color: #fff;
  font-size: 25px;
  background-color: #009688;
  border: none;
}

button:hover {
  cursor: pointer;
}

.modal-header {
  font-size: 30px;
}

.modal-body {
  font-size: 18px;
}

.modal-footer button {
  font-size: 15px;
  height: 30px;
}

.cancel-btn {
  background: #fff;
  color: #004D40;
  border: 1px solid #4DB6AC;
}

.input-group .control{
  height: 60px;
  padding: 0;
  box-sizing: border-box;
}
</style>
