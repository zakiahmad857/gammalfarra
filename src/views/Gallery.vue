<template>
  <div class="gallery">
    <decoration />
    <navigation />
    <div class="gallery__container">
      <h1 class="heading-0 mb-2">GALLERY</h1>
      <div class="galleries">
        <router-link :to="`/${state.lang}/gallery/pre-wedding`">
          <div class="gallery-item">
            <img src="../assets/images/artwork-canvas.svg" alt="pre-wedding" />
            <h2 class="heading-1">Pre-Wedding</h2>
          </div>
        </router-link>
        <router-link :to="`/${state.lang}/gallery/engagement`">
          <div class="gallery-item">
            <img src="../assets/images/artwork-canvas.svg" alt="engagement" />
            <h2 class="heading-1">Engagement</h2>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script>
import { reactive } from 'vue';
import Decoration from '../components/Decoration.vue';
import { useRoute } from 'vue-router';
import Navigation from '../components/Navigation.vue';

export default {
  name: 'Gallery',
  components: {
    Decoration,
    Navigation
  },
  setup() {
    const state = reactive({ lang: '' });

    const route = useRoute();

    route.path.startsWith('/id') ? (state.lang = 'id') : (state.lang = 'en');

    return { state };
  }
};
</script>
<style lang="scss" scoped>
@import '../scss/variables.scss';

.gallery {
  height: 100vh;
  background-color: $color-green-c;

  &__container {
    display: flex;
    flex-direction: column;
    padding-top: 25vh;
    align-items: center;
    position: relative;
    z-index: 999;
  }
}

.galleries {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 4rem;
}

.gallery-item {
  cursor: pointer;
  position: relative;

  transform: scale(1);
  transition: all 0.3s;

  &:hover {
    transform: scale(1.05);
  }

  &:active {
    transform: scale(0.95);
  }

  img {
    height: 20rem;
  }
}

.heading-0 {
  font-weight: 400;
  color: $color-blue;
}

.heading-1 {
  font-weight: 400;
  color: $color-black-2;
  font-size: 1.875rem;
  position: absolute;
  top: 46%;
  left: 50%;
  transform: translate(-50%, -50%);
  white-space: nowrap;
}
</style>
