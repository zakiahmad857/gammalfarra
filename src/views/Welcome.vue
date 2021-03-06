<template>
  <div class="welcome">
    <decoration />
    <div
      class="welcome-step-1"
      :class="{
        'fade-out': state.currentStep !== 'step-1' && state.currentStep !== ''
      }"
    >
      <div class="bg"></div>
      <div class="bg-1"></div>
      <img
        src="../assets/images/image-welcome.svg"
        alt="welcome"
        class="welcome-img"
        :class="{
          show: state.currentStep === 'step-1',
          hide: state.currentStep === ''
        }"
      />
    </div>
    <div
      :class="{ showed: state.currentStep === 'step-2' }"
      class="welcome-step-2"
    >
      <GreetingCard
        :lang="state.lang"
        @send="handleSend"
        :class="{
          showing: state.currentStep === 'step-2-showing',
          showed: state.currentStep === 'step-2',
          collapsing: state.currentStep === 'step-2-collapsing'
        }"
      />
    </div>
    <div
      :class="{ showed: state.currentStep === 'step-3' }"
      class="welcome-step-3"
    >
      <Thankyou
        :lang="state.lang"
        @done="handleDone"
        :class="{
          showing: state.currentStep === 'step-3-showing',
          showed: state.currentStep === 'step-3',
          collapsing: state.currentStep === 'step-3-collapsing'
        }"
      />
    </div>
  </div>
</template>

<script>
import { reactive } from 'vue';
import { useRouter, useRoute } from 'vue-router';
import { promiseTimeOut } from '../utils/promiseTimeOut';
import GreetingCard from '../components/GreetingCard.vue';
import Thankyou from '../components/Thankyou.vue';
import Decoration from '../components/Decoration.vue';

export default {
  name: 'Welcome',
  components: { GreetingCard, Thankyou, Decoration },
  setup() {
    const state = reactive({
      currentStep: '',
      lang: ''
    });
    const route = useRoute();
    const router = useRouter();

    route.path.startsWith('/id') ? (state.lang = 'id') : (state.lang = 'en');

    async function handleSend(_e) {
      state.currentStep = 'step-2-collapsing';
      await promiseTimeOut(1000);
      state.currentStep = 'step-3-showing';
      await promiseTimeOut(300);
      state.currentStep = 'step-3';
    }

    async function handleDone(_e) {
      state.currentStep = 'step-3-collapsing';
      await promiseTimeOut(500).then(() => {
        let lang;
        route.path.startsWith('/id') ? (lang = 'id') : (lang = 'en');
        router.push(`/${lang}/live-wedding`);
      });
    }

    return { state, handleSend, handleDone };
  },
  async mounted() {
    await promiseTimeOut(100);
    this.state.currentStep = 'step-1';
    await promiseTimeOut(3000);
    this.state.currentStep = 'step-2-showing';
    await promiseTimeOut(300);
    this.state.currentStep = 'step-2';
  }
};
</script>

<style lang="scss" scoped>
@import '../scss/variables.scss';

.welcome {
  height: 100vh;
  background: $color-green-c;
}

.welcome-img {
  height: 90vh;
  width: auto;
  transition: transform 1s ease-out;

  @media only screen and (max-width: 28.125em) {
    height: 70vh;
  }

  &.hide {
    transform: scale(0);
  }

  &.show {
    transform: scale(1);
  }
}

.welcome-step-1 {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  z-index: 9999;

  display: flex;
  justify-content: center;
  align-items: center;
  opacity: 1;

  transition: opacity 2s ease-out, blur 1s 2s;

  &.fade-out {
    opacity: 0;
    filter: blur(20px);
    z-index: -1;
  }

  .bg,
  .bg-1 {
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
  }

  .bg {
    background-color: $color-orange-c;
    z-index: -2;
  }

  .bg-1 {
    background: radial-gradient(
      108.79% 108.79% at 51.91% 51.95%,
      #fafafa 36.46%,
      rgba(255, 255, 255, 0) 100%
    );
    z-index: -1;
  }
}

.welcome-step-1,
.welcome-step-2,
.welcome-step-3 {
  position: fixed;
  top: 0;
  left: 0;
  height: 100%;
  width: 100%;

  &.showed {
    z-index: 100;
  }
}

.welcome-step-2 {
  justify-content: center;
  align-items: center;
  display: flex;
}

.welcome-step-3 {
  display: flex;
  justify-content: center;
  align-items: center;
}
</style>
