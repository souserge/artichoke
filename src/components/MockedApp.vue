<template>
  <div class="container">
    <div class="mocked-app">
      <div class="menu" @click="toggleMenu"></div>
      <transition name="slide">
        <div v-show="isMenuOpened" class="menu-dropdown">
          <ul>
            <li
              :key="idx"
              v-for="(dish, idx) in dishes"
              @click="selectMenuDish(idx)"
            >{{ idx + 1 }}. {{ dish.name }}</li>
          </ul>
        </div>
      </transition>
      <vue-displacement-slideshow
        class="dish"
        :images="dishSources"
        :intensity="0.2"
        :speedIn="0.4"
        :speedOut="0.4"
        ease="Expo.easeInOut"
        ref="slideshow"
      ></vue-displacement-slideshow>
      <div class="info">
        <div class="info__name">({{ currentDishIdx + 1 }}/{{ numDishes }}) {{ currentDish.name }}</div>
        <div class="info__desc">{{ currentDish.desc }}</div>
      </div>
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

function rollOver(x, a, b) {
  return x < a ? b : x > b ? a : x;
}

export default {
  components: {
    VueDisplacementSlideshow
  },
  data() {
    return {
      dishes: [
        {
          src: require("@/assets/dish-1.jpg"),
          name: "Rice with Salmon and veggies",
          desc: "This is a beautiful dish"
        },
        {
          src: require("@/assets/dish-2.jpg"),
          name: "Vegan choco pudding with fruit",
          desc: "This one is even better!"
        }
      ],

      currentDishIdx: 0,
      isMenuOpened: false
    };
  },

  computed: {
    dishSources() {
      return this.dishes.map(x => x.src);
    },

    numDishes() {
      return this.dishes.length;
    },

    currentDish() {
      return this.dishes[this.currentDishIdx];
    }
  },

  methods: {
    setImage(idx) {
      const cIdx = rollOver(idx, 0, this.numDishes - 1);
      this.currentDishIdx = cIdx;
      this.$refs.slideshow.next(cIdx);
    },

    changeDishLeft() {
      this.setImage(this.currentDishIdx - 1);
    },

    changeDishRight() {
      this.setImage(this.currentDishIdx + 1);
    },

    toggleMenu() {
      this.isMenuOpened = !this.isMenuOpened;
    },

    closeMenu() {
      this.isMenuOpened = false;
    },

    selectMenuDish(idx) {
      this.setImage(idx);
      this.closeMenu();
    }
  }
};
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
}

.mocked-app {
  color: white;
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

  .info {
    position: absolute;
    width: 90%;
    // height: 15%;
    // min-height: 50px;
    bottom: 1%;
    padding: 8px;
    background: rgba(0, 0, 0, 0.5);
    border: black solid 2px;
    border-radius: 25px;
    right: 50%;
    transform: translateX(50%);
    text-align: center;

    &__name {
      font-size: 1.2em;
      font-weight: 900;
    }

    &__desc {
      padding: 10px;
      text-align: left;
    }
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

    transition-property: filter;
    transition-duration: 0.2s;

    &:hover {
      filter: drop-shadow(0 0 10px rgba(0, 0, 0, 0.5));
    }
  }

  .menu {
    background-image: url("../assets/alacarte.svg");
    background-repeat: no-repeat;
    background-position: center;
    background-size: cover;
    cursor: pointer;
    width: 80px;
    height: 80px;
    position: absolute;
    top: 1%;
    left: 0;

    transition-property: filter;
    transition-duration: 0.2s;

    &:hover {
      filter: drop-shadow(2px 2px grey);
    }
  }

  .menu-dropdown {
    padding: 20px 10px;
    position: absolute;
    top: 70px;
    left: 0;
    background: rgba(0, 0, 0, 0.5);
    border: black solid;
    border-width: 2px 2px 2px 0;
    border-radius: 0 25px 25px 0;
    text-align: left;

    li {
      padding: 5px 3px;

      &:hover {
        cursor: pointer;
        background-color: rgba(lightgrey, 0.3);
      }
    }
  }
}

.slide-enter-active {
  transition: all 0.5s ease;
}
.slide-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-enter,
.slide-leave-to {
  transform: translateX(-100%);
}
</style>