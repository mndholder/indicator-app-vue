<template>
  <div id="app">
    <Indicator :current="value" :min="10" :max="120" />
  </div>
</template>

<script>
import Indicator from './components/Indicator.vue';

export default {
  name: "App",
  components: {
    Indicator,
  },
  data: () => ({
    value: 0,
  }),
  mounted() {
    this.$options.interval = setInterval(
        () => (this.value = Math.random() * 130),
        3000
    );
  },
  destroyed() {
    clearInterval(this.$options?.interval);
  },
};
</script>
