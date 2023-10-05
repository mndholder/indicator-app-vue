<template>
  <div class="Indicator">
    <div class="Indicator__Scale">
      <div
          class="Indicator__Arrow"
          :style="{ transform: `rotate(${getRotationValue()}deg)` }"
      />
    </div>
    <span class="Indicator__Value">{{ currentFormatted }}&deg;C</span>
  </div>
</template>

<script lang="ts">
// Sync angles with css
export const MIN_ANGLE = -45;
export const MAX_ANGLE = 225;

export default {
  props: {
    min: Number,
    max: Number,
    current: Number,
  },

  computed: {
    currentFormatted() {
      return Number(this.current).toFixed(0);
    },
  },

  methods: {
    getRotationValue() {
      const currentValue = Number(this.current) || 0;
      const minScaleValue = Number(this.min) || 0;
      const maxScaleValue = Number(this.max) || 0;

      if (currentValue <= minScaleValue) {
        return MIN_ANGLE;
      } else if (currentValue >= maxScaleValue) {
        return MAX_ANGLE;
      } else {
        const ANGLE_INTERVAL = Math.abs(MIN_ANGLE) + Math.abs(MAX_ANGLE);
        return (
            (ANGLE_INTERVAL * (currentValue - minScaleValue)) /
            (maxScaleValue - minScaleValue) +
            MIN_ANGLE
        );
      }
    },
  },
};
</script>

<style scoped lang="scss">
@use "sass:math";

$arrow-width: 3px;
$arrow-margin: 15px;
$scale-width: 15px;
$min-angle: -45deg;
$max-angle: 225deg;

.Indicator {
  position: relative;
  border-radius: 100%;
  border: black 1px solid;
  width: 100px;
  height: 100px;
  z-index: 10;

  &:before {
    content: "";
    display: block;
    position: absolute;
    height: $arrow-width;
    width: calc(50% + #{$scale-width});
    top: calc(50% - #{math.div($arrow-width, 2)});
    background-color: black;
    transform-origin: calc(100% - #{$scale-width}) 50%;
    transform: rotate($min-angle) translate(-$scale-width);
    z-index: 0;
  }

  &:after {
    content: "";
    display: block;
    position: absolute;
    height: $arrow-width;
    width: calc(50% + #{$scale-width});
    top: calc(50% - #{math.div($arrow-width, 2)});
    background-color: black;
    transform-origin: calc(100% - #{$scale-width}) 50%;
    transform: rotate($max-angle) translate(-$scale-width);
    z-index: 0;
  }

  &__Scale {
    background-color: white;
    position: absolute;
    display: block;
    width: 100%;
    height: 100%;
    border-radius: 100%;
    z-index: 1;
  }

  &__Arrow {
    display: block;
    position: absolute;
    height: $arrow-width;
    width: calc(50% - #{$arrow-margin});
    top: calc(50% - calc(#{math.div($arrow-width, 2)}));
    background-color: black;

    transform-origin: calc(100% + #{$arrow-margin}) 50%;
    transform: rotate(0deg);
    transition: transform 0.3s ease;
  }

  &__Value {
    display: block;
    width: 100%;
    position: absolute;
    top: 100%;
    text-align: center;
  }
}
</style>
