<template>
  <button
    class="scroll-indicator"
    ref="scrollIndicator"
  >
    <i
      v-for="(_, index) in [0,0,0]"
      :key="index"
    >
      ▽
    </i>
  </button>
</template>

<script>
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
gsap.registerPlugin(ScrollTrigger);

export default {
  mounted() {
    this.scrollIndicatorAnimation = this.animateScrollIndicator();
    this.fadeScrollIndicator();
  },
  methods: {
    animateScrollIndicator() {
      let scrollIndicators = this.$refs.scrollIndicator.children;

      return gsap
        .timeline({
          repeat: -1,
          scrollTrigger: {
            toggleActions: 'resume pause restart pause',
            start: '-10px',
            end: '50px',
          },
        })
        .to(null, { duration: 1.5 })
        .to(scrollIndicators, {
          y: 20,
          stagger: 0.1,
          duration: 0.4,
          ease: 'power1.in',
        })
        .to(scrollIndicators, {
          y: 0,
          stagger: 0.1,
          duration: 0.4,
          ease: 'power1.out',
          delay: -0.2,
        });
    },
    fadeScrollIndicator() {
      return gsap
        .timeline({
          paused: true,
          scrollTrigger: {
            trigger: 'document',
            id: 'scroll-indicator',
            start: 'top top',
            end: '50px',
            scrub: true,
          },
        })
        .to(this.$refs.scrollIndicator, { opacity: 0, duration: 0.1 });
    },
  },
};
</script>

<style lang="scss" scoped>
.scroll-indicator {
  align-items: center;
  background: transparent;
  border: none;
  color: white;
  display: flex;
  font-size: 3rem;
  height: 3rem;
  justify-content: center;
  left: 50%;
  position: fixed;
  top: 50%;
  transform: translate(-50%, -50%);
  user-select: none;
  width: 3rem;
  z-index: 99;

  i {
    position: absolute;
    font-style: normal;
    opacity: 0.4;

    &:first-of-type {
      opacity: 0.8;
    }
  }
}
</style>