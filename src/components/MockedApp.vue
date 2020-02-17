<template>
  <div class="container">
    <div class="mocked-app">
      <div class="menu" @click="toggleMenu"></div>
      <div class="order" @click="makeOrder">
        <p>ORDER</p>
      </div>
      <transition name="slide-right">
        <div v-show="isOrderComplete" class="order-complete">
          <p>{{ getOrderCompleteString() }}</p>
          <p class="order-complete__greeting">Bon Appétit!</p>
        </div>
      </transition>
      <transition name="slide-left">
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
        <div class="info__size">
          Portion size ({{currentDish.minSize}}–{{currentDish.maxSize}} grams):
          <input
            type="number"
            :min="currentDish.minSize"
            :max="currentDish.maxSize"
            v-model="dishSizeSelector"
            :step="10"
          /> g
        </div>
        <!-- <div class="info__desc">{{ currentDish.desc }}</div> -->
        <div class="info__ingredients">
          <v-dropdown>
            <template v-slot:label>Ingredients</template>
            <template v-slot:content>{{ getIngredientsString(currentDish) }}</template>
          </v-dropdown>
        </div>
        <div class="info__nutrition">
          <v-dropdown>
            <template v-slot:label>Nutrition</template>
            <template v-slot:content>
              <div class="nutrition-data">{{ getNutritionString(currentDish) }}</div>
            </template>
          </v-dropdown>
        </div>
      </div>
      <div class="arrow arrow--left" @click="changeDishLeft"></div>
      <div class="arrow arrow--right" @click="changeDishRight"></div>
    </div>
  </div>
</template>

<script>
import VueDisplacementSlideshow from "vue-displacement-slideshow";
import VDropdown from "./VDropdown";

// function constrain(x, a, b) {
//   return x < a ? a : x > b ? b : x;
// }

function rollOver(x, a, b) {
  return x < a ? b : x > b ? a : x;
}

export default {
  components: {
    VueDisplacementSlideshow,
    VDropdown
  },
  data() {
    return {
      dishes: [
        {
          src: require("@/assets/dish-1.jpg"),
          name: "Rice with Salmon and veggies",
          desc: "This is a beautiful dish",
          standardSize: 300,
          selectedSize: 300,
          minSize: 150,
          maxSize: 600,
          ingredients: [
            { text: "Rice" },
            { text: "salmon" },
            { text: "soybeans" },
            { text: "carrot" },
            { text: "lemon" }
          ],
          nutrition: {
            fat: 10,
            saturatedFat: 2,
            carbs: 30,
            sugar: 5,
            protein: 10,
            fiber: 5
          }
        },
        {
          src: require("@/assets/dish-2.jpg"),
          name: "Vegan choco pudding with fruit",
          desc: "This one is even better!",
          standardSize: 200,
          selectedSize: 200,
          minSize: 100,
          maxSize: 300,
          ingredients: [
            { text: "Soya pudding" },
            { text: "cocoa" },
            { text: "banana slices" },
            { text: "strawberry" },
            { text: "nuts" }
          ],
          nutrition: {
            fat: 10,
            saturatedFat: 1,
            carbs: 40,
            sugar: 8,
            protein: 20,
            fiber: 15
          }
        }
      ],

      currentDishIdx: 0,
      isMenuOpened: false,
      isOrderComplete: false,
      dishSizeSelector: null
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
      this.$refs.slideshow.next(idx);
    },

    changeDishLeft() {
      this.selectMenuDish(this.currentDishIdx - 1);
    },

    changeDishRight() {
      this.selectMenuDish(this.currentDishIdx + 1);
    },

    toggleMenu() {
      this.isMenuOpened = !this.isMenuOpened;
    },

    closeMenu() {
      this.isMenuOpened = false;
    },

    selectMenuDish(idx) {
      const cIdx = rollOver(idx, 0, this.numDishes - 1);
      this.currentDishIdx = cIdx;
      this.dishSizeSelector = this.dishes[cIdx].selectedSize;
      this.setImage(cIdx);
      this.closeMenu();
    },

    getIngredientsString(dish) {
      return dish.ingredients.map(x => x.text).join(", ") + ".";
    },

    getNutritionString({ nutrition }) {
      const cn = x => Math.round((x / 100) * this.dishSizeSelector);
      return `Fat......................${cn(nutrition.fat)} g
  of which saturated.....${cn(nutrition.saturatedFat)} g
Carbs....................${cn(nutrition.carbs)} g
  of which sugar.........${cn(nutrition.sugar)} g
Protein..................${cn(nutrition.protein)} g
Fiber....................${cn(nutrition.fiber)} g`;
    },

    makeOrder() {
      this.isOrderComplete = true;
      setTimeout(() => {
        this.isOrderComplete = false;
      }, 6000);
    },

    getOrderCompleteString() {
      const price = "1569";
      return `Ordered ${this.dishSizeSelector} grams of "${this.currentDish.name}" for €${price}.`;
    }
  },

  mounted() {
    this.dishSizeSelector = this.currentDish.selectedSize;
  }
};
</script>

<style lang="scss" scoped>
.container {
  width: 100%;
  background: black;
}

.mocked-app {
  color: white;
  max-width: 414px;
  max-height: 736px;
  width: 100vw;
  height: 100vh;
  margin: 0 auto;
  position: relative;
  overflow: hidden;

  .dish {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  .info {
    z-index: 9;
    position: absolute;
    width: 90%;
    bottom: 0;
    padding: 8px;
    background: rgba(0, 0, 0, 0.66);
    border: black solid;
    border-width: 2px 2px 0 2px;
    border-radius: 25px 25px 0 0;
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

    &__size {
      padding-top: 5px;

      input {
        font-size: 1.2em;
        width: 50px;
        color: white;
        border: 0;
        outline: 0;
        background: transparent;
        border-bottom: 1px solid white;
      }
    }

    &__size,
    &__ingredients,
    &__nutrition {
      margin-bottom: 7px;
    }

    &__nutrition {
      .nutrition-data {
        // text-align: center;
        white-space: pre;
        font-family: monospace;
        font-weight: 600;
        line-height: 1em;
        font-size: 1.2em;
      }
    }
  }

  .arrow {
    background-image: url("../assets/arrow.svg");
    background-repeat: no-repeat;
    background-position: center;
    background-size: 50px 50px;
    cursor: pointer;
    width: 60px;
    height: 50%;
    position: absolute;
    top: calc(50%);

    &--right {
      right: 0;
      transform: translateY(-50%);
    }

    &--left {
      left: 0;
      transform: translateY(-50%) rotate(180deg);
    }

    transition-property: filter;
    transition-duration: 0.2s;

    &:hover {
      background-color: rgba(lightgrey, 0.3);

      // filter: drop-shadow(0 0 10px rgba(0, 0, 0, 0.5));
    }
  }

  .menu {
    z-index: 11;
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
    z-index: 10;
    padding: 20px 10px;
    position: absolute;
    top: 80px;
    left: 0;
    background: rgba(0, 0, 0, 0.66);
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

  .order {
    z-index: 11;
    position: absolute;
    top: 1%;
    right: 2%;
    vertical-align: center;
    text-align: center;
    color: white;
    font-size: 1em;
    font-weight: 900;
    width: 60px;
    height: 60px;
    border: 5px solid black;
    background-color: rgba(0, 0, 0, 0.66);
    border-radius: 100%;
    cursor: pointer;

    line-height: 60px;

    transition-property: filter;
    transition-duration: 0.2s;

    &:hover {
      filter: drop-shadow(2px 2px grey);
    }
  }

  .order-complete {
    z-index: 12;
    padding: 20px 10px;
    text-align: left;
    background: rgba(0, 0, 0, 0.66);
    border: black solid;
    border-width: 2px 0 2px 2px;
    border-radius: 25px 0 0 25px;
    position: absolute;
    color: white;
    top: 80px;
    left: 0;
    margin-left: 20%;
    font-weight: bold;
    line-height: 1.1em;

    &__greeting {
      padding-top: 7px;
      text-align: right;
    }
  }
}

.slide-left-enter-active,
.slide-right-enter-active {
  transition: all 0.5s ease;
}
.slide-left-leave-active,
.slide-right-leave-active {
  transition: all 0.5s cubic-bezier(1, 0.5, 0.8, 1);
}
.slide-left-enter,
.slide-left-leave-to {
  transform: translateX(-100%);
}

.slide-right-enter,
.slide-right-leave-to {
  transform: translateX(100%);
}
</style>