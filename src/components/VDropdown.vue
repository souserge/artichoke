<template>
  <div class="dropdown" :class="isOpened ? 'dropdown--opened' : ''">
    <div class="dropdown__label" @click="toggle">
      <slot name="label"></slot>
      <div class="dropdown__icon"></div>
    </div>
    <div class="dropdown__content">
      <slot name="content"></slot>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      isOpened: false
    };
  },

  methods: {
    toggle() {
      this.isOpened = !this.isOpened;
    }
  }
};
</script>

<style lang="scss" scoped>
.dropdown {
  text-align: left;
  &__label {
    font-size: 1.3em;
    font-weight: bold;
    display: inline-block;
    margin-bottom: 0.3em;

    &:hover {
      background-color: rgba(lightgrey, 0.3);
      cursor: pointer;
    }
  }

  &__icon {
    display: inline-block;
    background: url("../assets/triangle.svg");
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
    height: 1rem;
    width: 1rem;
    transition-property: transform;
    transition-duration: 0.3s;
    transform: rotate(180deg) translateY(-0.1rem);
  }

  &__content {
    visibility: hidden;
    height: 0;
    overflow: hidden;
  }

  &--opened {
    .dropdown__content {
      visibility: visible;
      height: auto;
    }

    .dropdown__icon {
      transform: translateY(0.1rem);
    }
  }
}
</style>