<template>
  <div v-if="show" class="add-users">
    <ul class="users">
      <li v-for="(user, index) in users" v-bind:key="index">
        <input type="text" :ref="'user' + index" v-model="user.name" placeholder="Add a user's name..." autofocus>
        <a href="#" class="button add" @click="add(index)"><i class="fas fa-plus"></i></a>
        <a href="#" v-if="users.length > 1" class="button remove" @click="remove(index)"><i class="fas fa-minus"></i></a>
      </li>
      <li><button class="button done" :disabled="canSubmit" @click="done">Let's play!</button></li>
    </ul>
  </div>
</template>

<script>
import { EventBus } from '../bus.js'

export default {
  name: 'AddUsers',
  data () {
    return {
      users: [{ name: '' }, { name: '' }],
      show: true
    }
  },
  computed: {
    canSubmit () {
      return this.cleanUsers().length < 2
    }
  },
  methods: {
    add (index) {
      this.users.splice(index + 1, 0, {
        name: ''
      })
      this.$nextTick(() => {
        this.$refs[`user${index + 1}`][0].focus()
      })
    },
    remove (index) {
      this.users.splice(index, 1)
    },
    done () {
      EventBus.$emit('users', this.cleanUsers())
      this.show = false
    },
    cleanUsers () {
      return this.users.filter(value => value.name !== '')
    }
  },
  mounted () {
    EventBus.$on('showAddUsers', () => {
      this.show = true
    })
  }
}
</script>

<style scoped lang="scss">
@import '../assets/variables';

.add-users {
  position: absolute;
  background: $background;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

.users {
  list-style-type: none;
  width: 440px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);

  li {
    margin-bottom: 10px;
  }
}

input {
  background: $box;
  border: 0;
  color: $type;
  border-radius: 24px;
  font-size: 16px;
  padding: 10px 20px;
  width: 300px;
  outline: none;
}

.button {
  display: inline-block;
  width: 38px;
  height: 38px;
  line-height: 38px;
  text-align: center;
  border-radius: 36px;
  vertical-align: middle;
  color: $background;
  margin-left: 10px;
  transition: background 250ms ease-in-out;
}

.add {
  background: $good;

  &:hover {
    background: darken($good, 10%);
  }
}

.remove {
  background: $bad;

  &:hover {
    background: darken($bad, 20%);
  }
}

.done {
  width: 100%;
  margin-left: 0;
  margin-top: 30px;
  border: 0;
  text-transform: uppercase;
  font-size: 16px;
  background: $reset;
  transition: background 250ms ease-in-out;

  &:hover {
    background: darken($reset, 20%);
  }

  &[disabled] {
    opacity: 0.4;
  }
}
</style>
