<template>
  <div>
    <div class="wrap">
      <div class="container" :class="{active}" ref="container">
        <div
          class="inventory"
          v-for="i in quantity"
          :key="i">
          {{ i }}
        </div>
      </div>
      <div class="divider" :class="{finished}"></div>
    </div>
    <div class="buttons">
      <button type="button" @click="start">Start</button>
      <button type="button" @click="clear">Clear</button>
    </div>
  </div>
</template>

<script>
import gsap from 'gsap';

export default {
  data() {
    return {
      duration: 8,
      winPosition: 45,
      startPosition: 3,
      width: 150,
      margin: 30,
      border: 3,
      quantity: 50,
      active: false,
      timeline: null,
      sound: new Audio('roulette_spin.mp3'),
      finished: false,
    };
  },
  computed: {
    round() {
      return this.width + this.border * 2 + this.margin;
    },
    translate() {
      const positions = this.winPosition - this.startPosition;
      return this.round * positions;
    },
  },
  methods: {
    start() {
      if (this.timeline) this.clear();
      const sounds = new Array(this.winPosition - this.startPosition).fill('', 0).map((_, i) => {
        const sound = this.sound.cloneNode();
        return {
          sound,
          time: this.round * i + (this.width / 2 + this.border + this.margin),
        };
      });
      this.timeline = gsap.timeline();
      this.timeline.to(this.$refs.container, this.duration, {
        x: -this.translate,
        onUpdate: function onUpdate() {
          // eslint-disable-next-line no-underscore-dangle
          const el = this._targets[0];
          let { transform } = el.style;
          transform = transform.match(/\((.+)\)/);
          const x = -parseFloat(transform[1].split(',')[0]);
          if (sounds.length && x >= sounds[0].time) {
            const sound = sounds.shift();
            sound.sound.play();
          }
        },
        onComplete: () => {
          this.finished = true;
        },
      });
    },
    clear() {
      this.timeline.progress(0);
      this.timeline.kill();
      this.finished = false;
    },
  },
};
</script>

<style lang="scss">
$height: 150px;
$width: 150px;
$padding: 10px;
$margin: 30px;
$quantity: 50;
$border: 3px;

.wrap {
  position: relative;
  width: ($width + $border * 2) * 5 + $margin * 4;
  display: inline-block;
  margin: auto;
  border: $border solid;
  height: $height;
  padding: $padding + $border $padding;
  overflow: hidden;
}
.container {
  position: absolute;
  top: $padding;
  left: $padding;
  display: inline-block;
  width: ($width + $border * 2 + $margin) * $quantity;
  height: $height;
  text-align: left;
}
.inventory {
  position: relative;
  display: inline-block;
  height: $height;
  width: $width;
  border: $border solid red;
  margin-right: $margin;
  text-align: center;
  font-size: 80px;
  line-height: $height;
  color: #8080801a;
  font-weight: 900;
}
.buttons {
  margin-top: 100px;
  text-align: center;
  button {
    margin: 0 15px;
  }
}
.divider {
  display: inline-block;
  position: absolute;
  height: 100%;
  left: calc(50% - 1px);
  top: 0;
  border: 2px solid violet;
  &.finished {
    height: 146px;
    top: initial;
  }
}
</style>
