<template>
  <input
    class="input"
    @input="handleChange"
    type="text"
    :name="name"
    v-model="state.value"
    :class="[this.class, { active: state.value.length > 0 }]"
    :placeholder="this.placeholder"
  />
</template>

<script>
import { reactive } from 'vue';

export default {
  name: 'Input',
  props: {
    value: String,
    name: String,
    class: String,
    placeholder: String
  },
  setup(_props, ctx) {
    const state = reactive({ value: '' });

    function handleChange() {
      ctx.emit('on-change', state.value);
    }

    return { state, handleChange };
  }
};
</script>

<style lang="scss" scoped>
@import '../scss/variables.scss';

.input {
  border: none;
  outline: none;
  background-color: transparent;
  border-bottom: 1px solid $color-black-3;
  padding: 0.875rem;
  letter-spacing: 1px;
  font-family: 'Darker Grotesque', sans-serif;
  font-size: 2rem;
  text-align: center;
  color: $color-orange;
  transition: all 0.3s;

  &.active {
    border-bottom: 1px solid $color-orange;
  }
}
</style>
