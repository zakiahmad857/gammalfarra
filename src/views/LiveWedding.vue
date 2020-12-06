<template>
  <div class="live-wedding">
    <div v-if="state.blur" class="overlay-blur"></div>
    <div
      :class="{
        collapsing: state.souvenir === 'collapsing',
        showing: state.souvenir === 'showing',
        showed: state.souvenir === 'showed',
        collapsed: state.souvenir === 'collapsed'
      }"
      class="popup-souvenir"
    >
      <Souvenir @close="handleClose('souvenir')" />
    </div>
    <div
      :class="{
        collapsing: state.amplop === 'collapsing',
        showing: state.amplop === 'showing',
        showed: state.amplop === 'showed',
        collapsed: state.amplop === 'collapsed'
      }"
      class="e-amplop"
    >
      <Amplop :lang="state.lang" @close="handleClose('amplop')" />
    </div>
    <div class="utils">
      <div id="step-6" @click.prevent="this.startTour" class="util">
        <img src="../assets/icons/icon-info.svg" alt="info" />
      </div>
      <div id="step-5" class="util">
        <img
          v-if="!isPlay"
          @click="this.playMusic"
          src="../assets/icons/icon-sound-on.svg"
          alt="sound-on"
        />
        <img
          v-else
          @click="this.stopMusic"
          src="../assets/icons/icon-sound-off.svg"
          alt="sound-off"
        />
        <div style="display: none">
          <vue-plyr ref="plyr">
            <div class="plyr__video-embed">
              <iframe
                width="560"
                height="315"
                src="https://www.youtube.com/embed/gIB2egm7tL8?autoplay=1"
                frameborder="0"
                allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
                allowfullscreen
              ></iframe>
            </div>
          </vue-plyr>
        </div>
      </div>
    </div>
    <nav class="navigation">
      <router-link :to="`/${state.lang}/gallery`">
        <img
          id="step-2"
          class="nav-item"
          src="../assets/images/nav-gallery.svg"
          alt="gallery"
        />
      </router-link>
      <img
        id="step-3"
        @click="handleClose('souvenir')"
        class="nav-item nav-item--souvenir"
        src="../assets/images/nav-souvenir.svg"
        alt="souvenir"
      />
      <div style="position: relative">
        <img
          @click="handleClose('amplop')"
          id="step-4"
          class="nav-item"
          src="../assets/images/nav-amplop.svg"
          alt="amplop"
        />
        <img
          class="img-pot"
          src="../assets/images/image-pot.svg"
          alt="amplop"
        />
      </div>
    </nav>
    <div id="step-1" class="video">
      <div class="video__container">
        <img
          src="../assets/images/wedding-gamma-alfarra.png"
          alt="Wedding Gamma Alfarra"
          class="video__text"
        />
        <vue-plyr ref="videoPlyr">
          <div class="plyr__video-embed">
            <iframe
              width="560"
              height="315"
              src="https://www.youtube.com/embed/4m7A3i5CR-g?rel=0&amp;autoplay=1&mute=1"
              frameborder="0"
              allow="accelerometer; autoplay; muted; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
              allowfullscreen
            ></iframe>
          </div>
        </vue-plyr>
      </div>
    </div>
    <tour ref="tour" :lang="state.lang" :steps="steps" />
  </div>
</template>

<script>
import { reactive, computed } from 'vue';
import { useRoute } from 'vue-router';
import Cookies from 'js-cookie';
import store from '../store';
import Souvenir from '../components/Souvenir.vue';
import Amplop from '../components/Amplop.vue';
import { promiseTimeOut } from '../utils/promiseTimeOut';
import Tour from '../components/Tour.vue';
import TourMultiLang from '../json/Tour.json';

export default {
  name: 'LiveWedding',
  components: { Souvenir, Amplop, Tour },
  setup() {
    const state = reactive({
      routeBefore: '',
      lang: '',
      souvenir: 'collapsed',
      amplop: 'collapsed',
      blur: false
    });

    const steps = computed(() => [
      {
        header: TourMultiLang[state.lang]['step-1'].header,
        content: TourMultiLang[state.lang]['step-1'].content,
        position: 'bottom',
        target: '#step-1',
        modifiers: [
          {
            name: 'offset',
            options: {
              offset: [0, -40]
            }
          },
          {
            name: 'preventOverflow',
            options: {
              altAxis: true
            }
          }
        ]
      },
      {
        header: TourMultiLang[state.lang]['step-2'].header,
        content: TourMultiLang[state.lang]['step-2'].content,
        position: 'top-end',
        target: '#step-2'
      },
      {
        header: TourMultiLang[state.lang]['step-3'].header,
        content: TourMultiLang[state.lang]['step-3'].content,
        position: 'top',
        target: '#step-3'
      },
      {
        header: TourMultiLang[state.lang]['step-4'].header,
        content: TourMultiLang[state.lang]['step-4'].content,
        position: 'top-start',
        target: '#step-4'
      },
      {
        header: TourMultiLang[state.lang]['step-5'].header,
        content: TourMultiLang[state.lang]['step-5'].content,
        position: 'bottom-end',
        target: '#step-5',
        modifiers: [
          {
            name: 'offset',
            options: {
              offset: [20, 20]
            }
          }
        ]
      },
      {
        header: TourMultiLang[state.lang]['step-6'].header,
        content: TourMultiLang[state.lang]['step-6'].content,
        position: 'bottom-end',
        target: '#step-6',
        modifiers: [
          {
            name: 'offset',
            options: {
              offset: [20, 20]
            }
          }
        ]
      }
    ]);

    const isPlay = computed(() => store.state.isPlay);

    const route = useRoute();
    route.path.startsWith('/id') ? (state.lang = 'id') : (state.lang = 'en');

    async function handleClose(name) {
      if (state[name] === 'collapsed') {
        state[name] = 'showing';
        await promiseTimeOut(100);
        state[name] = 'showed';
        state.blur = true;
      } else if (state[name] === 'showed') {
        state[name] = 'collapsing';
        await promiseTimeOut(300);
        state.blur = false;
        state[name] = 'collapsed';
      }
    }

    return { state, handleClose, isPlay, steps };
  },
  mounted() {
    const cookie = Cookies.get('isFinishedTutorial');
    if (!cookie) {
      this.startTour();
    }

    if (this.isPlay) {
      setTimeout(() => {
        this.playMusic();
      }, 100);
    }

    const player = this.$refs.videoPlyr.player;

    player.on('play', () => {
      this.stopMusic();
    });
  },
  methods: {
    startTour() {
      this.$refs.tour.start();
    },
    playMusic() {
      this.$refs.plyr.player.play();
      store.dispatch('playMusic');
    },
    stopMusic() {
      this.$refs.plyr.player.stop();
      store.dispatch('stopMusic');
    }
  }
};
</script>

<style lang="scss" scoped>
@import '../scss/variables.scss';
.live-wedding {
  background: url('../assets/images/bg-live-wedding.png');
  background-size: cover;
  background-position: center;
  height: 100vh;
  display: flex;
  overflow: hidden;
  flex-direction: column;
}

.video {
  height: auto;
  width: 45%;
  position: absolute;
  top: 49%;
  left: 50%;
  transform: translate(-50%, -50%);

  @media only screen and (max-width: 64em) and (orientation: portrait) {
    width: 70%;
  }

  @media only screen and (max-width: 28.175em) {
    width: 85%;
  }

  &:hover {
    z-index: 999;
  }

  &__text {
    position: absolute;
    top: -8rem;
    left: -10rem;
    height: 15rem;
    z-index: 10;

    @media only screen and (max-width: 28.175em) {
      left: 50%;
      transform: translateX(-50%);
      top: -15rem;
    }
  }

  &__container {
    border-radius: 1.5rem;
    overflow: hidden;
    border: 10px solid $color-green-c;
    height: 100%;
    width: 100%;
  }
}

.popup-souvenir,
.e-amplop {
  position: fixed;
  height: 100%;
  width: 100%;
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1050;
  transform: scale(0);
  opacity: 0;
  transition: all 0.3s;

  &.collapsed {
    display: none;
  }

  &.showing,
  &.collapsing {
    display: flex;
  }

  &.showed {
    transform: scale(1);
    opacity: 1;
  }
}

.img-pot {
  position: absolute;
  top: -30rem;
  left: -18rem;
  height: 40rem;
  z-index: 0;
}

.navigation {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 100%;
  padding: 0 1.5rem;
  height: 244px;
  margin-top: auto;
  position: relative;

  @media only screen and (max-width: 43.75em) {
    & > div {
      position: absolute !important;
      right: 2rem;
      bottom: 8rem;

      .nav-item {
        bottom: 0;
        right: 0;
        position: relative;
      }
    }
  }
}

.nav-item {
  height: 15rem;
  cursor: pointer;
  position: relative;
  z-index: 1;
  transform: scale(1);
  transition: all 0.3s;

  @media only screen and (max-width: 43.75em) {
    position: absolute;

    &:first-child {
      left: 1rem;
      bottom: 8rem;
    }

    &:nth-child(2) {
      left: 50%;
      bottom: 0;
      transform: translateX(-50%) scale(1);

      &:active {
        transform: translateX(-50%) scale(0.95);
      }
    }
  }

  @media (hover: hover) {
    &:hover {
      transform: scale(1.05);
    }
  }

  &:active {
    transform: scale(0.95);
  }

  &--souvenir {
    margin-bottom: -2rem;
    display: block;
  }
}

.utils {
  display: flex;
  align-items: center;
  justify-content: flex-end;
  padding-top: 2rem;
  position: relative;
}

.util {
  display: flex;
  justify-content: center;
  align-items: center;
  background-color: $color-green-c;
  border-radius: 50%;
  cursor: pointer;
  margin-right: 1.4rem;
  z-index: 5;
  transform: scale(1);
  transition: all 0.3s;

  &:hover {
    transform: scale(1.05);
  }

  &:active {
    transform: scale(0.95);
  }

  img {
    height: 4rem;
  }
}
</style>
