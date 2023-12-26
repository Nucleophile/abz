<template>
  <HeaderBlock />
  <MainBlock />
  <GetRequest :usersReloadFlag="usersReloadFlag" />
  <PostRequest @successful-registration="usersReloadFlag++" />
</template>

<script>
import { ref } from "vue";
import HeaderBlock from "./components/HeaderBlock.vue";
import MainBlock from "./components/MainBlock.vue";
import GetRequest from "./components/GetRequest.vue";
import PostRequest from "./components/PostRequest.vue";

export default {
  name: "App",
  setup() {
    const usersReloadFlag = ref(0);

    return { usersReloadFlag };
  },
  components: {
    HeaderBlock,
    MainBlock,
    GetRequest,
    PostRequest,
  },
};
</script>

<style lang="scss">
*,
::before,
::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}
ul {
  list-style: none;
}
a {
  text-decoration: none;
}
:root {
  --container-max-width: none;
  @include media(from_tablet) {
    --container-max-width: #{$tablet-container};
  }
  @include media(from_laptop) {
    --container-max-width: #{$laptop-container};
  }
  @include media(desktop) {
    --container-max-width: #{$desktop-container};
  }
}
body {
  font-family: "Nunito", sans-serif;
  @include defaultFont;
  background: $page-bg;
  color: $fontColor;
}
article {
  margin-bottom: rem(100);
}
h1 {
  font-size: rem(40);
  line-height: rem(40);
  margin-bottom: rem(50);
  text-align: center;
}
.container {
  max-width: var(--container-max-width);
  margin: 0 auto;
  padding: 0 $container-gutter;
}
.content-small {
  max-width: $small-content;
  margin: 0 auto;
}
.btn {
  display: inline-block;
  @include defaultFont;
  padding: rem(4) rem(18);
  min-width: rem(100);
  border: none;
  border-radius: rem(34);
  color: $fontColor;
  text-align: center;
  &:hover {
    cursor: pointer;
  }
  &--default {
    background: $primary;
    &:hover,
    &:focus-visible {
      background: $primary-hover;
    }
  }
  &[disabled] {
    background: $primary-disabled;
    cursor: not-allowed;
  }
}
.ta-center {
  text-align: center;
}
.get-request,
.post-request {
  padding-top: rem(40);
}
</style>
