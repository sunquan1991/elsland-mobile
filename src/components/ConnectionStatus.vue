<template>
    <transition-group tag="div" class="wrapper" name="fade" mode="out-in">
      <h1 class="centered" v-show="stage === 1" key="1">Connecting...</h1>

      <div class="text" v-show="stage >= 2" key="2">

        <transition-group tag="div" name="fade" mode="out-in">
          <div v-show="stage === 2" key="1">
            <img class="welcome" src="../assets/images/connection-status-welcome.svg" />
            <p class="start button" @click="start">Start</p>
          </div>

          <p class="instruction" v-show="stage === 3" key="2">
            Most of the experience will be on the BIG SCREEN.
            <br />
            <br />
            When you notice small particles on the screen, look back onto your phone.
          </p>
        </transition-group>
        <transition name="entrance-animation" delay="5000">
          <img v-show="stage <= 4" class="entrance" src="../assets/images/connection-status-entry.png" />
        </transition>
        <transition name="silhouette-animation">
          <img v-show="stage <= 4" class="silhouette" src="../assets/images/connection-status-silhouette.png" />
        </transition>
      </div>
    </transition-group>
</template>

<script>
export default {
  name: 'ConnectionStatus',
  props: {
    goToScene: Function,
    development: Boolean
   },
  watch: {
    stage: function(val) {
      // welcome
      if (val === 2 && !this.development) {
        setTimeout(() => { this.stage = 3 }, 5000)
      }
      if (val === 3 && this.development) {
        setTimeout(() => { this.stage = 4 }, 6000)
      }
      if (val === 4) {
        setTimeout(() => { this.stage = 5 }, 1000)
      }
      if (val === 5) {
        setTimeout(() => { this.stage = 6 }, 8000)
      }
      if (val === 6) {
        setTimeout(() => { this.goToScene("ONBOARDING") })
      }
    }
  },
  methods: {
    start() {
      this.stage = 4
    }
  },
  sockets: {
    connect() {
      this.stage = 2
    },
    intro_scene() {
      this.stage = 4
    },
  },
  data() {
    return {
      stage: 1
    }
  },
  mounted() {
    setTimeout(() => { this.development && this.stage++ }, 1000)
  }
}
</script>

<style scoped>
.wrapper {
  background: url("../assets/images/connection-status-bg.png");
  background-size: cover;
  height: 100%;
}

.wrapper.fade-leave-to {
  opacity: 1;
}

.text {
  position: relative;
  height: 100%;
  width: 100%;
}

.welcome, .instruction, .entrance, .silhouette {
  left: 50%;
  transform: translateX(-50%);
  position: absolute;
}

.welcome {
  width: 55%;
  z-index: 3;
  top: 35%;
  transform: translate(-50%, -50%);
}

.start {
  left: 50%;
  position: absolute;
  transform: translate(-50%, -50%);
  top: 46.5%;
}

.instruction {
  width: 70%;
  top: 25%;
  font-size: 1.2rem;
  z-index: 10;
}

.entrance {
  transition: all 2s ease-in 5s;
  transform-origin: center center;
  z-index: 1;
  width: 77%;
  bottom: 0;
  animation: blink 1.5s infinite alternate;
}

.silhouette {
  z-index: 9;
  width: 32%;
  bottom: 0;
}

.entrance-animation-leave-active {
  transform: translateX(-50%) scaleX(0.01);
}

.silhouette-animation-leave-active {
  animation: silhouette-animation 5s ease-in;
}

@media only screen and (max-height: 720px) {
  .instruction {
    font-size: 1rem;
    top: 25%;
    transform: translate(-50%, -50%);
    margin: 0;
  }
}

@keyframes blink {
  0%  {
    opacity: 1;
  }
  5%  {
    opacity: 0.9;
  }
  7%  {
    opacity: 0.8;
  }
  9%  {
    opacity: 0.7;
  }
  11%  {
    opacity: 0.8;
  }
  15%  {
    opacity: 1;
  }
}

@keyframes silhouette-animation {
  0%  {
    transform: translateX(-50%) translateY(0) scale(1);

  }
  30% {
    opacity: 100%;
  }
  100% {
    transform: translateX(-50%) translateY(-80px) scale(0.2);
    opacity: 0%;
  }
}
</style>
