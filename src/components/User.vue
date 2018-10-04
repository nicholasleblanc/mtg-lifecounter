<template>
  <div class="user" :class="{ 'user--dead': isDead }">
    <div class="inner">
      <div class="name">{{ name }}</div>
      <div class="score">
        {{ score }}
      </div>

      <ul class="actions actions--left">
        <li><a href="#" @click="changeScore(1)" class="good">1</a></li>
        <li><a href="#" @click="changeScore(5)" class="good">5</a></li>
        <li><a href="#" @click="changeScore(10)" class="good">10</a></li>
      </ul>

      <ul class="actions actions--right">
        <li><a href="#" @click="changeScore(-1)" class="bad">1</a></li>
        <li><a href="#" @click="changeScore(-5)" class="bad">5</a></li>
        <li><a href="#" @click="changeScore(-10)" class="bad">10</a></li>
      </ul>

      <div v-if="score == 0" class="lost">
        <div class="lost__copy">
          {{ randomDeadCopy }}
        </div>

        <a href="#" @click="revertScore" class="lost__whoops">
          Whoops, clicked the wrong button
        </a>
      </div>
    </div>
  </div>
</template>

<script>
import { EventBus } from '../bus.js'

export default {
  name: 'User',
  props: {
    name: String
  },
  data () {
    return {
      score: 0,
      lastScore: 0,
      deadCopy: [
        'Not your day. Better luck next time.'
      ],
      randomDeadCopy: ''
    }
  },
  computed: {
    isDead () {
      return this.score === 0
    }
  },
  methods: {
    changeScore (adjustment) {
      this.lastScore = this.score
      this.score = this.score + adjustment

      if (this.score < 0) {
        this.score = 0
      }
    },
    revertScore () {
      this.score = this.lastScore
    },
    getRandomDeadCopy () {
      this.randomDeadCopy = this.deadCopy[Math.floor(Math.random() * this.deadCopy.length)]
    }
  },
  mounted () {
    EventBus.$on('reset', score => {
      this.score = score
      this.getRandomDeadCopy()
    })
  }
}
</script>

<style scoped lang="scss">
@import '../assets/variables';

.user {
  flex: 0 0 auto;
  width: 50%;
  padding: 20px;
}

.user--dead {
  opacity: 0.3;
}

.inner {
  position: relative;
  background: $box;
  height: 100%;
}

.name {
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  text-align: center;
  font-size: 30px;
  text-transform: uppercase;
  padding-bottom: 260px;
}

.score {
  position: absolute;
  top: 50%;
  left: 50%;
  margin-top: -10px;
  transform: translate(-50%, -50%);
  font-weight: bold;
  font-size: 234px;
}

.actions {
  position: absolute;
  top: 50%;
  list-style-type: none;
  margin: 0;
  padding: 0;
  left: 50%;
  transform: translate(-50%, -50%);

  li {
    margin: 10px;

    a {
      display: block;
      position: relative;
      z-index: 1;
      color: $type;
      text-decoration: none;
      text-align: center;
      width: 60px;
      height: 60px;
      line-height: 60px;
      border-radius: 100%;
      font-weight: bold;
      transition: background 250ms ease-in-out;

      &:before {
        display: inline;
        font-size: 24px;
        vertical-align: middle;
        position: relative;
        padding-right: 5px;
        opacity: 0.7;
      }

      &.good {
        background: $good;

        &:hover {
          background: darken($good, 10%);
        }

        &:before {
          content: '+';
          top: -2px;
        }
      }

      &.bad {
        background: $bad;

        &:hover {
          background: darken($bad, 20%);
        }

        &:before {
          content: '-';
          top: -3px;
        }
      }
    }
  }
}

.actions--left {
  margin-left: -235px;
}

.actions--right {
  margin-left: 235px;
}

.lost {
  position: absolute;
  font-size: 50px;
  background: $box;
  width: 100%;
  height: 100%;
  text-align: center;
}

.lost__copy {
  position: relative;
  top: 50%;
  transform: translateY(-50%);
  padding: 50px;
}

.lost__whoops {
  position: absolute;
  display: inline-block;
  bottom: 50px;
  left: 50%;
  transform: translateX(-50%);
  background: $background;
  border-radius: 24px;
  font-size: 16px;
  padding: 10px 20px;
  color: $type;
  text-decoration: none;
  text-transform: uppercase;
}
</style>
