<template>
  <div class="intro">
    <div class="intro__header">
      <img class="logo" src="@/assets/logo.png" />
      <h1 class="title">GreenAdvisor</h1>
    </div>
    <div class="intro__description">Hello! We're GreenAdvisor! We're a startup that is blablabal.</div>
    <div class="intro__image">
      <img alt="Instructions" src="../assets/instructions.jpg" />
    </div>
    <div class="intro__footer">
      <h2>Try out our mockup below 👇</h2>
    </div>
  </div>
</template>

<script>
export default {};
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
