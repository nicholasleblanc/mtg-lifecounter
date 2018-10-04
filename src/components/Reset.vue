<template>
  <div class="reset">
    <div v-if="show" class="reset-modal">
      <ul class="types">
        <li class="type" @click="reset(40)">
          <div class="type__icon"><i class="fas fa-crown"></i></div>
          <div class="type__title">Commander</div>
        </li>

        <li class="type" @click="reset(20)">
          <div class="type__icon"><i class="fas fa-align-justify"></i></div>
          <div class="type__title">Standard</div>
        </li>
      </ul>

      <a href="#" v-if="loaded" @click="show = false" class="cancel">
        Crap, don't reset
        </a>
    </div>

    <a href="#" v-if="!show" @click="show = true" class="reset-trigger">
      <i class="fas fa-sync-alt"></i>
    </a>
  </div>
</template>

<script>
import { EventBus } from '../bus.js'

export default {
  name: 'Reset',
  data () {
    return {
      show: true,
      loaded: false
    }
  },
  methods: {
    reset (amount) {
      EventBus.$emit('reset', amount)
      this.show = false
      this.loaded = true
    }
  },
  mounted () {
    EventBus.$on('showReset', () => {
      this.show = true
    })
  }
}
</script>

<style scoped lang="scss">
@import '../assets/variables';

.reset-modal {
  position: absolute;
  background: $background;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

.types {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  list-style-type: none;
  margin: 0;
  padding: 0;
}

.type {
  background: $reset;
  text-align: center;
  width: 300px;
  padding: 50px;
  border-radius: 20px;
  color: $background;
  float: left;
  margin: 20px;
  cursor: pointer;
  transition: background 250ms ease-in-out;

  &:hover {
    background: darken($reset, 20%);
  }
}

.type__icon {
  font-size: 50px;
}

.type__title {
  font-size: 30px;
  text-transform: uppercase;
}

.cancel {
  position: absolute;
  display: inline-block;
  bottom: 50px;
  left: 50%;
  transform: translateX(-50%);
  background: $box;
  border-radius: 24px;
  font-size: 16px;
  padding: 10px 20px;
  color: $type;
  text-decoration: none;
  text-transform: uppercase;
}

.reset-trigger {
  position: fixed;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  display: block;
  background: $reset;
  text-align: center;
  width: 120px;
  height: 120px;
  line-height: 112px;
  border-radius: 100%;
  color: $background;
  font-size: 50px;
  transition: background 250ms ease-in-out;

  &:hover {
    background: darken($reset, 20%);
  }
}
</style>
