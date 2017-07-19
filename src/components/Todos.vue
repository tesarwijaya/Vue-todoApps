<template>
  <div>
    <div class="row">
      <div class="col-m-4">
        <div class="dropdown-control category-control">
          <div class="active" @click="toggleShowCatList()">{{activeCategory === null ? 'Select' : activeCategory}} <i class="material-icons dropdown-indicator-icon" style="float:right; line-height:54px;">keyboard_arrow_left</i></div>
          <div class="dropdown-menu-container" v-show="showCatList">
            <ul class="dropdown-list">
              <li v-for="category in categories" @click="changeActiveCategory(category['.value'])"><span>{{category['.value']}}</span></li>
              <li><input type="text" name="" placeholder="Add new category" v-model="newCategory" @keyup.enter="submitCategory"></li>
            </ul>
          </div>
        </div>
      </div>
      <div class="col-m-8">
        <input class="todo-input" type="text" name="" placeholder="Input new task then press enter" @keyup.enter="submitTask" v-model="newTask">
      </div>
    </div>
    <div class="row">
      <div class="col-m-12">
        <div class="filter-container">
          <label for="filter">filter by</label>
          <select id="filter" class="filter-selector" v-model="currentFilter">
            <option value="All">All</option>
            <option :value="category['.value']" v-for="category in categories">{{category['.value']}}</option>
          </select>
        </div>
      </div>
    </div>
    <div class="row">
      <div class="col-m-12">
        <ul class="todo-list">      
          <li class="todo-item" v-for="task in displayedTask" :key="task['.key']">{{task['subject']}}<i class="material-icons close-btn" @click="removeTask(task)">close</i></li>
        </ul>
      </div>
    </div>
    <div style="clear:both"></div>
  </div>
</template>
<script>
import Vue from 'vue'
import {firebaseApp} from '../firebase'
import VueFire from 'vuefire'
import VueLocalStorage from 'vue-ls'
import _ from 'lodash'

Vue.use(VueFire)
Vue.use(VueLocalStorage)

const db = firebaseApp.database()
export default {
  name: 'Todos',
  firebase: function () {
    return {
      tasks: this.userTaskRef,
      categories: this.userCatRef
    }
  },
  created () {
    firebaseApp.auth().onAuthStateChanged((user) => {
      if (!user) {
        this.$router.push({name: 'AppIndex'})
      }
    })
  },
  computed: {
    displayedTask: function () {
      if (this.currentFilter !== 'All') {
        return _.filter(this.tasks, ['category', this.currentFilter])
      } else {
        return this.tasks
      }
    }
  },
  methods: {
    submitTask: function () {
      if (this.newTask !== null && this.activeCategory !== null) {
        this.userTaskRef.push({subject: this.newTask, category: this.activeCategory})
        this.newTask = null
        this.activeCategory = null
      } else {
        alert('dont leave category and task empty :)')
      }
    },
    submitCategory: function () {
      if (this.newCategory !== null) {
        this.userCatRef.push(this.newCategory)
        this.newCategory = null
      } else {
        alert('Please name new category')
      }
    },
    removeTask: function (task) {
      this.userTaskRef.child(task['.key']).remove()
    },
    changeActiveCategory: function (category) {
      this.activeCategory = category
      this.toggleShowCatList()
    },
    toggleShowCatList: function () {
      this.showCatList = !this.showCatList
    }
  },
  data () {
    return {
      msg: 'Todos component',
      newTask: null,
      newCategory: null,
      activeCategory: null,
      showCatList: false,
      currentFilter: 'All',
      userTaskRef: db.ref('users/' + firebaseApp.auth().currentUser.uid + '/todos/'),
      userCatRef: db.ref('users/' + firebaseApp.auth().currentUser.uid + '/categories/')
    }
  }
}
</script>
<style scoped>
.todo-input {
  height: 60px;
  margin: 0;
  width: 100%;
}

.todo-list {
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.todo-item {
  position: relative;
  height: 60px;
  line-height: 60px;
  padding-left: 10px;
  padding-right: 10px;
  border-bottom: 2px solid #c1c4c9;
}

.close-btn {
  opacity: 0;
  visibility: hidden;
  position: absolute;
  right: 10px;
  line-height: 60px;
  font-size: 30px;
  cursor: pointer;
  -webkit-transition: all 100ms cubic-bezier(0.4, 0.0, 1, 1); /* Safari */
  transition: all 100ms cubic-bezier(0.4, 0.0, 1, 1);
}

.todo-item:hover .close-btn {
  opacity: 1;
  visibility: visible;
}

.category-control {
  line-height: 54px;
  height: 60px;
  box-sizing: border-box;
  padding: 0 10px;
  border: 1px solid rgb(238, 238, 238);
}

.category-control .active {
  font-weight: bold;
}
.category-control .active div {
  overflow:hidden;
  text-overflow: ellipsis;
}
.dropdown-control {
  position: relative;
  text-overflow: ellipsis;
}

.dropdown-control:focus .dropdown-menu-container {
  visibility: visible;
  opacity: 1;
}

.dropdown-control:focus .active .dropdown-indicator-icon {
  -ms-transform: rotate(-90deg); /* IE 9 */
  -webkit-transform: rotate(-90deg); /* Chrome, Safari, Opera */
  transform: rotate(-90deg);
  -ms-transition: all 100ms cubic-bezier(0.4, 0.0, 1, 1);
  -webkit-transition: all 100ms cubic-bezier(0.4, 0.0, 1, 1); /* Safari */
  transition: all 100ms cubic-bezier(0.4, 0.0, 1, 1);
}

.dropdown-menu-container {
  position: absolute;
  top: 60px;
  min-width: 250px;
  z-index: 3;
  background: #fff;
  box-shadow: 0 3px 6px rgba(0,0,0,0.16), 0 3px 6px rgba(0,0,0,0.23);
}

.dropdown-menu-container ul {
  margin:0;
  padding: 0;
  list-style-type: none;
}

.dropdown-menu-container ul li {
  cursor: pointer;
}
.dropdown-menu-container ul li span {
  margin-left: 10px;
}
.dropdown-menu-container ul li:hover {
  background: #edeff2;
}

.filter-container {
  padding-left: 10px;
  border-bottom: 2px solid #c1c4c9;
}

.filter-container label {
  font-size: 24px;
  margin-bottom: 10px;
}

.filter-selector {
  height: 24px;
  font-size: 24px;
  border: none;
}
</style>
