<template>
  <div class="login">
    <decoration />
    <div class="login__container">
      <h1 class="heading-0">
        {{ state.multiLang[state.lang].welcome }}
      </h1>
      <h2 class="mb-1">{{ state.multiLang[state.lang].instruction }}</h2>
      <Input
        class="mb-5"
        :placeholder="state.multiLang[state.lang].placeholder"
        @on-change="handleChange"
      />
      <Button @click="handleLogin" class="mb-5">{{
        state.lang === 'id' ? 'Masuk' : 'Login'
      }}</Button>
      <div class="langs">
        <router-link to="/id">
          <div @click="state.lang = 'id'" class="lang">
            <img
              class="lang-ico"
              src="../assets/icons/icon-id.svg"
              :class="{ inactive: state.lang === 'en' }"
            />
            <div
              class="shadow-inset"
              :class="{ show: state.lang === 'en', id: state.lang === 'en' }"
            ></div>
          </div>
        </router-link>
        <router-link style="position: relative;" to="/en">
          <div @click="state.lang = 'en'" class="lang">
            <img
              class="lang-ico"
              src="../assets/icons/icon-en.svg"
              :class="{ inactive: state.lang === 'id' }"
            />
            <div
              class="shadow-inset"
              :class="{ show: state.lang === 'id', en: state.lang === 'id' }"
            ></div>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
// @ is an alias to /src
import { useRoute, useRouter } from 'vue-router';
import Button from '@/components/Button.vue';
import Input from '@/components/Input.vue';
import { reactive } from 'vue';
import Decoration from '../components/Decoration.vue';
import LoginMultiLang from '../json/Login.json';

export default {
  name: 'Login',
  components: {
    Button,
    Input,
    Decoration
  },
  setup() {
    const route = useRoute();
    const router = useRouter();
    const state = reactive({
      lang: 'id',
      inputVal: '',
      multiLang: LoginMultiLang
    });

    if (route.path.startsWith('/en')) state.lang = 'en';

    function handleChange(val) {
      state.inputVal = val;
    }

    function handleLogin() {
      router.push(`/${state.lang}/welcome`);
    }

    return { handleChange, state, handleLogin };
  }
};
</script>

<style lang="scss" scoped>
@import '../scss/variables.scss';

.login {
  height: 100vh;
  background-color: $color-green-c;
  &__container {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding-top: 30vh;
    position: relative;
    z-index: 1;

    @media only screen and (max-width: 90em) and (orientation: landscape) {
      padding-top: 20vh;

      .heading-0 {
        font-size: 5rem;
      }
    }

    @media only screen and (max-width: 28.125em) {
      padding: 0 2rem;
      padding-top: 25vh;
    }
  }
}

h1,
h2,
.mb-5 {
  @media only screen and (max-width: 28.125em) {
    order: 2;
  }
}

h1,
h2 {
  text-align: center;
}

h1 {
  color: $color-blue-a;
}

h2 {
  font-family: 'Darker Grotesque', sans-serif;
  font-size: 2.25rem;
  color: $color-blue-b;
  font-weight: 400;

  @media only screen and (max-width: 90em) and (orientation: landscape) {
    font-size: 2rem;
  }
}

.lang-ico {
  width: 5rem;
  cursor: pointer;
  position: relative;
  display: inline-block;
  box-shadow: 5px 0px 5px rgba(0, 0, 0, 0.2);

  @media only screen and (max-width: 28.125em) {
    width: 7rem;
  }

  &.inactive {
    opacity: 0.3;
  }
}

.shadow-inset {
  height: 92%;
  width: 100%;
  position: absolute;
  top: 0;
  left: 0;
  display: none;
  box-shadow: inset 0px 4px 4px rgba(0, 0, 0, 0.25),
    inset 4px -4px 4px rgba(0, 0, 0, 0.1);

  &.en {
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
  }

  &.id {
    border-top-left-radius: 5px;
    border-bottom-left-radius: 5px;
  }

  &.show {
    display: block;
  }
}

.lang {
  position: relative;
}

.langs {
  display: flex;
  align-items: center;

  @media only screen and (max-width: 28.125em) {
    order: 1;
    margin-bottom: 2rem;
  }
}
</style>
