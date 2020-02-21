<template>
  <div class="intro">
    <div class="intro__header">
      <img class="logo" src="@/assets/logo.png" />
      <h1 class="title">GreenAdvisor</h1>
    </div>
    <div class="intro__description">
      <p>{{ introText }}</p>
      <p style="font-weight: bold">{{ introTextBold }}</p>
    </div>
    <div class="intro__image">
      <img alt="Instructions" src="../assets/instructions.jpeg" />
    </div>
    <div class="intro__footer">
      <h2>Try out our mockup below 👇</h2>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      introText: `Hello! We're GreenAdvisor! We have an idea of an Augmented Reality phone application that allows restaurant customers to see how the dishes in the menu look like and customize size and ingredients. This would allow for a more conscious customer decision that can yield a decrease in the food wasted in the restaurants.`,
      introTextBold: `Please help us validate our idea by trying our mockup and leaving feedback below!`
    };
  }
};
</script>

<style lang="scss" scoped>
.intro {
  width: 75%;
  margin: 10px auto;
  img {
    width: 100%;
    height: auto;
    object-fit: cover;
  }

  display: grid;
  grid-template-areas:
    "header header"
    "description image"
    "footer footer";
  grid-template-columns: 1fr 1fr;
  grid-template-rows: 60px auto auto;

  @media only screen and (max-width: 600px) {
    width: 100%;
    grid-template-columns: 1fr;
    grid-template-rows: 60px auto auto auto;
    grid-template-areas:
      "header"
      "description"
      "image"
      "footer";

    .intro__description {
      padding-left: 1em;
      padding-right: 1em;
    }
  }

  grid-column-gap: 1em;
  grid-row-gap: 2em;

  &__header {
    grid-area: header;
    vertical-align: middle;

    .logo {
      height: 50px;
      width: auto;
      object-fit: contain;

      transition-property: filter;
      transition-duration: 0.3s;
      transform: translateX(5px);

      &:hover {
        filter: sepia(450%) saturate(450%) hue-rotate(45deg);
      }
    }

    .title {
      display: inline;
      font-size: 3em;
      font-weight: 900;
      color: rgb(91, 212, 91);
    }
  }
  &__description {
    grid-area: description;
  }

  &__image {
    grid-area: image;
  }

  &__footer {
    grid-area: footer;
    text-align: center;
  }
}
</style>
