<template>
  <button
    class="btn"
    :class="[this.class, { 'btn--grey': this.color === 'grey' }]"
  >
    <slot />
  </button>
</template>

<script>
export default {
  name: 'Button',
  props: {
    class: String,
    color: String
  }
};
</script>

<style lang="scss" scoped>
@import '../scss/variables.scss';
.btn {
  background: #ede6dc;
  border: 1px solid #ede6dc;
  box-sizing: border-box;
  box-shadow: -1px -3px 6px 6px #f8f3ed, 3px 4px 10px 5px #c1aa94;
  border-radius: 25px;
  padding: 0.625rem 1.25rem;
  text-align: center;
  cursor: pointer;
  color: #d97e56;
  min-width: 12.5rem;
  outline: none;
  font-size: 1.25rem;
  font-family: 'Darker Grotesque', sans-serif;

  &--grey {
    background: #f6f5f5;
    box-shadow: 0px -3px 6px 6px #fefdfc, 3px 4px 10px 5px #e0d1d1;
    color: $color-green-a;
    font-weight: 700;
  }

  &:active {
    box-shadow: inset 0px -3px 7px 2px #f8f3ed, inset 3px 4px 9px 2px #c1aa94;
  }
}
</style>
