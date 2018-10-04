<template>
  <div id="app">
    <div class="users-list">
      <User v-for="(user, index) in users" v-bind:key="index" :name="user.name"/>
    </div>

    <Reset v-if="users.length"></Reset>

    <AddUsers></AddUsers>
  </div>
</template>

<script>
import AddUsers from './components/AddUsers.vue'
import Reset from './components/Reset.vue'
import User from './components/User.vue'
import { EventBus } from './bus.js'

export default {
  name: 'app',
  components: {
    AddUsers,
    Reset,
    User
  },
  data () {
    return {
      users: []
    }
  },
  mounted () {
    EventBus.$on('users', users => {
      this.users = users
    })
  }
}
</script>

<style lang="scss">
@import './assets/variables';

html, body {
  padding: 0;
  margin: 0;
  height: 100%;
  background: $background;
  color: $type;
  user-select: none;
}

* {
  box-sizing: border-box;
}

#app {
  font-family: 'Oswald', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100%;
  position: relative;
}

.users-list {
  display: flex;
  flex-wrap: wrap;
  height: 100%;
}
</style>
