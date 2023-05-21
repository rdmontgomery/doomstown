<template>
  <div class="container">
    <div v-if="timeRemaining > 0" class="timer">
      <h1>Jamestown Lady Problems Doomsday Timer</h1>
      <div>
        <span class="value">{{ days }}</span>
        <span class="label">Days</span>
      </div>
      <div>
        <span class="value">{{ hours }}</span>
        <span class="label">Hours</span>
      </div>
      <div>
        <span class="value">{{ minutes }}</span>
        <span class="label">Minutes</span>
      </div>
      <div>
        <span class="value">{{ seconds }}</span>
        <span class="label">Seconds</span>
      </div>
    </div>
    <div v-else class="timer">
      <h1>Well done</h1>
      <img src="@/assets/verticaljamestown.jpg" alt="Countdown Completed!"
        :style="`transform: rotate(${rotation}deg) scale(${scale})`" height="400" />
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      countdown: null,
      countdownDate: new Date('June 22, 2023 22:00:00 GMT-0500'), // Set the countdown date and time
      timeRemaining: 0,
      rotation: -90,
      rotationSpeed: .0000001,
      scale: 0.9
    };
  },
  computed: {
    days() {
      return Math.floor(this.timeRemaining / (60 * 60 * 24));
    },
    hours() {
      return Math.floor((this.timeRemaining % (60 * 60 * 24)) / (60 * 60));
    },
    minutes() {
      return Math.floor((this.timeRemaining % (60 * 60)) / 60);
    },
    seconds() {
      return this.timeRemaining % 60;
    },
  },
  mounted() {
    const now = new Date();
    const endTime = this.countdownDate.getTime();
    if (endTime > now) {
      this.timeRemaining = Math.floor((endTime - now.getTime()) / 1000);
      this.countdown = setInterval(() => {
        if (this.timeRemaining > 0) {
          this.timeRemaining--;
        } else {
          clearInterval(this.countdown);
        }
      }, 1000);
    }
    setInterval(() => {
      this.rotation += this.rotationSpeed;
      if (this.rotationSpeed < 5) {
        this.rotationSpeed += .0001;
      }
    }, 1);
  },
  beforeUnmount() {
    clearInterval(this.countdown);
  },
};
</script>

<style scoped>
.container {
  text-align: center;
  margin-top: 50px;
}

.timer {
  font-size: 36px;
  margin-top: 20px;
}

.timer > div {
  display: inline-block;
  margin: 0 10px;
}

.value {
  font-size: 48px;
}

.label {
  font-size: 14px;
}

.completed {
  font-size: 24px;
  color: green;
}
</style>
