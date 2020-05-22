<template>
  <section>
    <MsmLogo />
  </section>
</template>

<script>
import MsmLogo from './components/MsmLogo.vue';

export default {
  name: 'App',
  components: {
    MsmLogo
  }
};
</script>

<style lang="scss">
html,
body {
  padding: 0;
  margin: 0;
}

* {
  box-sizing: border-box;
}
</style>
