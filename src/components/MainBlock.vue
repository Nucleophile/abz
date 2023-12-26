<template>
  <article class="main-block">
    <div class="container">
      <div class="main-block__content">
        <div class="main-block__text">
          <h1 class="main-block__heading">
            Test assignment for front-end developer
          </h1>
          <p class="main-block__p">
            What defines a good front-end developer is one that has skilled
            knowledge of HTML, CSS, JS with a vast understanding of User design
            thinking as they'll be building web interfaces with accessibility in
            mind. They should also be excited to learn, as the world of
            Front-End Development keeps evolving.
          </p>
          <a href="#sign-up" class="btn btn--default">Sign up</a>
        </div>
      </div>
    </div>
  </article>
</template>

<script>
export default {
  name: "MainBLock",
};
</script>

<style lang="scss">
.main-block {
  &__content {
    color: #fff;
    padding: rem(40) 0;
    background: url(../assets/main-bg.jpg);
    background-size: cover;
    background-position: center;
    text-align: center;
    @include media(from_tablet) {
      padding: rem(88) 0;
    }
    @include media(from_laptop) {
      padding: rem(164) 0;
    }
    @include media(to_laptop) {
      margin-left: -$container-gutter * 2;
      margin-right: -$container-gutter * 2;
    }
    @include media(mobile) {
      margin-left: -$container-gutter;
      margin-right: -$container-gutter;
    }
  }
  &__text {
    max-width: $small-container;
    margin: 0 auto;
    padding: 0 $container-gutter;
  }
  &__heading {
    margin-bottom: rem(21);
  }
  &__p {
    margin-bottom: rem(32);
  }
}
</style>
