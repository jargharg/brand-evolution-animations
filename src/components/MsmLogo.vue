<template>
  <section
    class="msm-logo__section"
    ref="section"
  >
    <div
      class="msm-logo__container"
      ref="container"
    >
      <div
        class="msm-logo__logo"
        ref="logo"
      >
        <svg
          v-for="(element, index) in logoElements"
          :key="index"
          viewBox="0 0 130 130"
          xmlns="http://www.w3.org/2000/svg"
          width="100%"
          height="100%"
        >
          <path
            :d="element.path"
            :fill="element.fill"
          />
        </svg>
      </div>

      <div
        class="msm-logo__text"
        ref="text"
      >
        <msm-text />
      </div>

      <div
        class="msm-logo__scroll-indicator"
        ref="scrollIndicator"
      >
        <i
          v-for="(_, index) in [0,0,0]"
          :key="index"
        >
          ▽
        </i>
      </div>
    </div>
  </section>
</template>

<script>
import gsap from 'gsap';
import MsmText from './MsmText';

export default {
  components: {
    MsmText,
  },
  data() {
    return {
      logoElements: [
        {
          path: `M112.446 35.867c17.36 28.377 8.346 65.434-20.03 82.795-28.378 
          17.36-65.435 8.346-82.795-20.031-17.36-28.378 7.9-55.308 20.03-82.795 
          12.13-27.487 65.435-8.346 82.795 20.031z`,
          fill: '#ebcae9',
        },
        {
          path: `M112.224 57.122c6.009 26.708-10.795 53.305-37.614 59.314-26.82
           6.01-53.305-10.795-59.314-37.614-6.01-26.819 20.81-41.063 37.614-59.314 
           16.692-18.139 53.304 10.906 59.314 37`,
          fill: '#dda5d9',
        },
        {
          path: `M112.224 57.122c6.009 26.708-10.795 53.305-37.614 59.314-26.82
           6.01-53.305-10.795-59.314-37.614-6.01-26.819 20.81-41.063 37.614-59.314
            16.692-18.139 53.304 10.906 59.314 37`,
          fill: '#c86dc2',
        },
        {
          path: `M96.978 48.33c9.014 19.141.779 41.954-18.362 50.968-19.14
           9.014-41.954.78-50.967-18.361-9.014-19.141 8.79-34.498 18.361-50.968
            9.57-16.47 41.954-.779 50.968 18.362z`,
          fill: '#bc4bb4',
        },
        {
          path: `M96.978 48.33c9.014 19.141.779 41.954-18.362 50.968-19.14
           9.014-41.954.78-50.967-18.361-9.014-19.141 8.79-34.498 18.361-50.968
            9.57-16.47 41.954-.779 50.968 18.362z`,
          fill: '#a33d9c',
        },
      ],
    };
  },
  mounted() {
    let { section, container, logo } = this.$refs;

    this.timelineHeight = section.scrollHeight - container.scrollHeight;
    this.elements = Array.from(logo.children);
    this.timeline = this.createTimeline();

    this.animateScrollIndicator();

    window.addEventListener('scroll', this.progressTimeline.bind(this));
  },
  methods: {
    animateScrollIndicator() {
      let scrollIndicators = this.$refs.scrollIndicator.querySelectorAll('i');

      return gsap
        .timeline({ repeat: -1 })
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
    createTimeline() {
      let { section, container, logo, text, scrollIndicator } = this.$refs;
      let characters = this.$refs.text.querySelectorAll('path');
      let scale = section.offsetWidth / logo.offsetWidth + 3;
      let { x, y } = logo.getBoundingClientRect();

      gsap.set(characters, { opacity: 0 });
      gsap.set(logo, {
        x: (container.offsetWidth - logo.offsetWidth - x) / 2,
        y: (container.offsetHeight - logo.offsetHeight - y) / 2,
      });

      this.elements.forEach((element, index) => {
        gsap.set(element, {
          scale: scale + index * 0.7,
          opacity: 0.3,
          rotate: Math.random() * 360,
        });
      });

      let hideScrollIndicator = () =>
        gsap.timeline().to(scrollIndicator, { opacity: 0, duration: 0.1 });

      let morphToDonk = () =>
        gsap
          .timeline()
          .addLabel('start')
          .to(this.elements[4], { x: 0, y: -50, scale: 0.55 }, 'start')
          .to(this.elements[3], { x: 30, y: 30, scale: 0.55 }, 'start')
          .to(this.elements[2], { x: -45, y: -15, scale: 0.4 }, 'start')
          .to(this.elements[1], { x: -30, y: 35, scale: 0.4 }, 'start')
          .to(this.elements[0], { x: 45, y: -20, scale: 0.35 }, 'start')
          .to(this.elements, { opacity: 1, rotate: 0 }, 'start');

      let rotateDonk = () =>
        gsap
          .timeline()
          .addLabel('start')
          .to(logo, { rotate: 360, ease: 'sine.inOut' }, 'start')
          .to(this.elements, { rotate: -360, ease: 'sine.inOut' }, 'start')
          .set(this.elements, { rotate: 0 });

      let morphToLogo = () => {
        return gsap
          .timeline()
          .addLabel('start')
          .to(this.elements, { scale: 1, x: 0, y: 0 }, 'start')
          .to(logo, { x: 0, y: 0, ease: 'sine.inOut' }, 'start');
      };

      let fadeInText = () =>
        gsap.timeline().to(characters, {
          opacity: 1,
          stagger: 0.008,
          duration: 0.2,
        });

      return gsap
        .timeline({ paused: true })
        .addLabel('start')
        .add(hideScrollIndicator(), 'start')
        .add(morphToDonk(), 'start')
        .add(rotateDonk())
        .add(morphToLogo())
        .add(fadeInText(), '-=0.1');
    },
    progressTimeline() {
      let scrollTop = document.documentElement.scrollTop;
      let progress = scrollTop / this.timelineHeight;

      this.timeline.progress(progress);
    },
  },
};
</script>

<style scoped lang="scss">
.msm-logo {
  &__logo {
    width: 90px;
    height: 90px;
    position: relative;

    > svg {
      position: absolute;
      width: 100%;
    }
  }

  &__text {
    height: 90px;
    padding: 10px;

    > svg {
      height: 100%;
    }
  }

  &__scroll-indicator {
    align-items: center;
    color: white;
    display: flex;
    font-size: 3rem;
    height: 3rem;
    justify-content: center;
    left: 50%;
    position: absolute;
    top: 50%;
    transform: translate(-50%, -50%);
    user-select: none;
    width: 3rem;

    i {
      position: absolute;
      font-style: normal;
      opacity: 0.4;

      &:first-of-type {
        opacity: 0.8;
      }
    }
  }

  &__section {
    width: 100vw;
    height: 300vh;
    position: relative;
  }

  &__container {
    width: 100vw;
    position: sticky;
    top: 0;
    height: 100vh;
    overflow: hidden;
    display: flex;
    padding: 1rem;
  }
}
</style>
