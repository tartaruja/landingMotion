<template>
  <div ref="leniswrapper" :class="'uid' + dataUid">
    <div ref="VideoComponent" class="VideoComponent">
      <video
        src="https://assets.codepen.io/39255/output_960.mp4"
        playsinline="true"
        webkit-playsinline="true"
        preload="auto"
        muted="muted"
        class="video-wheat"
      ></video>
      <div class="textblock first">
        <div class="container-fluid">
          <div class="row d-flex flex-column">
            <h2
              class="col-12 offset-sm-8 offset-md-8 col-sm-4 text-color-secondary-item col-md-4 typo-w-4"
            >
              {{ dataFirstBlockTitle }}
            </h2>
            <p
              class="col-12 col-sm-4 col-md-4 offset-sm-8 offset-md-8 typo-a-6"
            >
              <span v-html="dataFirstBlockCopy"></span>
            </p>
          </div>
        </div>
      </div>
      <div class="textblock second">
        <div class="container-fluid">
          <div class="row d-flex flex-column">
            <h2
              class="col-12 col-sm-6 col-md-4 typo-w-4 text-color-secondary-item"
            >
              {{ dataSecondBlockTitle }}
            </h2>
            <p class="col-12 col-sm-6 col-md-4 typo-a-6">
              <span v-html="dataSecondBlockCopy"></span>
            </p>
          </div>
        </div>
      </div>
      <div class="textblock third">
        <div class="container-fluid">
          <div class="row d-flex flex-column">
            <h2
              class="col-12 col-sm-8 offset-sm-1 offset-md-2 text-color-secondary-item col-md-4 typo-w-4"
            >
              {{ dataThirdBlockTitle }}
            </h2>
            <p
              class="col-12 col-sm-8 col-md-4 typo-a-6 offset-sm-1 offset-md-2"
            >
              <span v-html="dataThirdBlockCopy"></span>
            </p>
          </div>
        </div>
      </div>
      <div class="textblock fourth">
        <div class="container-fluid">
          <div class="row d-flex flex-column pt-10">
            <h2
              class="col-12 col-sm-8 col-md-4 offset-sm-2 offset-md-3 typo-w-4 text-color-secondary-item"
            >
              {{ dataFourthBlockTitle }}
            </h2>
            <p
              class="col-12 col-sm-8 col-md-4 offset-sm-2 offset-md-3 typo-a-6"
            >
              <span v-html="dataFourthBlockCopy"></span>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { gsap } from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';
import Lenis from '@studio-freight/lenis';

gsap.registerPlugin(ScrollTrigger);

export default {
  name: 'VideoComponent',
  props: {
    dataUid: {
      type: String,
      default: '000',
    },
    dataFirstBlockTitle: {
      type: String,
      default: 'first title',
    },
    dataFirstBlockCopy: {
      type: String,
      default: 'first <b>copy</b>',
    },
    dataSecondBlockTitle: {
      type: String,
      default: 'Second title',
    },
    dataSecondBlockCopy: {
      type: String,
      default: 'second <b>copy</b>',
    },
    dataThirdBlockTitle: {
      type: String,
      default: 'Third title',
    },
    dataThirdBlockCopy: {
      type: String,
      default: 'Third <b>copy</b>',
    },
    dataFourthBlockTitle: {
      type: String,
      default: 'Fourth title',
    },
    dataFourthBlockCopy: {
      type: String,
      default: 'Fourth <b>copy</b>',
    },
  },
  created() {
    this.$nextTick(() => {
      /* LENIS SETUP (smooth scrolling feature) */
      let lenis;
      lenis = new Lenis({ lerp: 0.01 });

      function raf(time) {
        lenis.raf(time);
        requestAnimationFrame(raf);
      }

      requestAnimationFrame(raf);
      /* ---------------------------------- */
      /* Setup video */
      const video = document.querySelector(
        '.uid' + this.dataUid + ' .video-wheat'
      );
      // eslint-disable-next-line @typescript-eslint/no-unused-vars
      let src = `https://assets.codepen.io/39255/output_960.mp4`;
      function once(el, event, fn, opts) {
        var onceFn = function () {
          el.removeEventListener(event, onceFn);
          fn.apply(this, arguments);
        };
        el.addEventListener(event, onceFn, opts);
        return onceFn;
      }

      once(document.documentElement, 'touchstart', function () {
        video.play();
        video.pause();
      });
      

      /* ---------------------------------- */
      /* video ScrollTrigger implementation */
      /* uses callbacks to activate/deactvate lenis (lerp value: 1 = no smooth) */

      gsap.registerPlugin(ScrollTrigger);
      let tl = gsap.timeline({
        defaults: { duration: 1 },
        scrollTrigger: {
          trigger: video,
          start: 'top top',
          end: 'bottom+=700% bottom',
          scrub: true,
          pin: true,
          snap: {
            snapTo: [0, 0.35, 0.6, 0.9], // snap to the closest label in the timeline
            duration: { min: 0.2, max: 3 }, // the snap animation should be at least 0.2 seconds, but no more than 3 seconds (determined by velocity)
            delay: 0.2, // wait 0.2 seconds from the last scroll event before doing the snapping
            ease: 'power1.inOut', // the ease of the snap animation ("power3" by default)
          },
          /*
          onEnter: () => (lenis.options.lerp = 0.05),
          onLeave: () => (lenis.options.lerp = 1),
          onEnterBack: () => (lenis.options.lerp = 0.05),
          onLeaveBack: () => (lenis.options.lerp = 1),
          */
        },
      });

      /* video ScrollTrigger Timeline */
      once(video, 'loadedmetadata', () => {
        tl.fromTo(
          video,
          {
            currentTime: 0,
          },
          {
            currentTime: video.duration || 1,
          }
        );
      });

      /* text animations setup */
      this.setupShowTextAnim('.uid' + this.dataUid + ' .first');
      this.setupShowTextAnim('.uid' + this.dataUid + ' .second');
      this.setupShowTextAnim('.uid' + this.dataUid + ' .third');
      this.setupShowTextAnim('.uid' + this.dataUid + ' .fourth');
    });
  },
  methods: {
    setupShowTextAnim(classText) {
      gsap.to(classText, {
        autoAlpha: 1,
        scrollTrigger: {
          trigger: classText,
          start: 'center center',
          end: 'bottom top',
          pin: true,
          scrub: true,
        },
      });
    },
  },
};
</script>

<style lang="scss" scoped>
@import 'node_modules/bootstrap/scss/functions';
@import 'node_modules/bootstrap/scss/variables';
@import 'node_modules/bootstrap/scss/mixins';
.VideoComponent {
  height: 800vh;
  position: relative;
  .video-wheat {
    width: 100% !important;
    height: 100vh !important;
    object-fit: cover !important;
    position: absolute;
    top: 0;
    left: 0;
  }

  .textblock {
    width: 100%;
    height: 775px;
    display: flex;
    align-items: center;
    justify-content: center;
    color: rgb(0, 0, 0);
    font-size: 8rem;
    visibility: hidden;
    z-index: 9;
    &.first {
      visibility: visible;
      @include media-breakpoint-down(md) {
        padding-top: 300px;
      }
    }
    &.second > div {
      padding-top: 400px;
      @include media-breakpoint-down(md) {
        padding-top: 300px;
      }
    }
    &.third > div {
      padding-top: 400px;
      @include media-breakpoint-down(md) {
        padding-top: 100px;
      }
    }
    &.fourth > div {
      padding-top: 400px;
      @include media-breakpoint-down(md) {
        padding-top: 100px;
      }
    }
    h2 {
      font-weight: 400;
    }
    p {
      font-weight: 300;
    }
  }
}

/* LENIS STYLES */
html.lenis {
  height: auto;
}

.lenis.lenis-smooth {
  scroll-behavior: auto;
}

.lenis.lenis-smooth [data-lenis-prevent] {
  overscroll-behavior: contain;
}

.lenis.lenis-stopped {
  overflow: hidden;
}

.lenis.lenis-scrolling iframe {
  pointer-events: none;
}
</style>