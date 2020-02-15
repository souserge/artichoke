<template>
  <div class="container">
    <div class="mocked-app">
      <div class="menu"></div>
      <vue-displacement-slideshow
        class="dish"
        :images="images"
        displacement="require('../assets/displacement.png')"
        :intensity="0.2"
        :speedIn="0.4"
        :speedOut="0.4"
        ease="Expo.easeInOut"
        ref="slideshow"
      ></vue-displacement-slideshow>
      <div class="arrow arrow--left" @click="changeDishLeft"></div>
      <div class="arrow arrow--right" @click="changeDishRight"></div>
    </div>
  </div>
</template>

<script>
import VueDisplacementSlideshow from "vue-displacement-slideshow";

// function constrain(x, a, b) {
//   return x < a ? a : x > b ? b : x;
// }

export default {
  components: {
    VueDisplacementSlideshow
  },
  data() {
    return {
      current: 0
    };
  },

  computed: {
    images() {
      return [require("@/assets/dish-1.jpg"), require("@/assets/dish-2.jpg")];
    },
    numDishes() {
      return this.images.length;
    }
  },

  methods: {
    setImage(idx) {
      this.$refs.slideshow.next(idx);
    },

    changeDishLeft() {
      this.$refs.slideshow.previous();
    },

    changeDishRight() {
      this.$refs.slideshow.next();
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
}

.mocked-app {
  max-width: 414px;
  max-height: 736px;
  width: 100vw;
  height: 100vh;
  margin: 0 auto;
  position: relative;

  .dish {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .arrow {
    background-image: url("../assets/arrow.svg");
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
    cursor: pointer;
    width: 50px;
    height: 50px;
    position: absolute;
    top: calc(50% - 20px);

    &--right {
      right: 2%;
    }

    &--left {
      left: 2%;
      transform: rotate(180deg);
    }

    &:hover {
      width: 53px;
      height: 53px;
    }
  }

  .menu {
    background-image: url("../assets/hamburger.svg");
    background-repeat: no-repeat;
    background-position: center;
    background-size: contain;
    cursor: pointer;
    width: 50px;
    height: 50px;
    position: absolute;
    top: 1%;
    left: 2%;
  }
}
</style>