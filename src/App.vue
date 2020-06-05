<template>
  <section>
    <a
      class="github-link"
      href="https://github.com/jargharg/brand-evolution-animations"
      target="_blank"
    >github repo</a>
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

.github-link {
  position: fixed;
  bottom: 0;
  left: 0;
  padding: 1rem;
  color: white;
  mix-blend-mode: difference;
  z-index: 100;
}
</style>
