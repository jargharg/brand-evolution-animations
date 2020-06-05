<template>
  <section>
    <MsmLogo />
    <ScrollIndicator />
    <Content />
  </section>
</template>

<script>
import MsmLogo from './components/MsmLogo.vue';
import Content from './components/Content.vue';
import ScrollIndicator from './components/ScrollIndicator.vue';

export default {
  name: 'App',
  components: {
    MsmLogo,
    Content,
    ScrollIndicator,
  },
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
  font-family: monospace;
}
</style>
