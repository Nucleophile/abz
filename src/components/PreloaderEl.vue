<template>
  <div class="preloader">
    <img class="preloader__img" src="../assets/preloader.svg" alt="Preloader" />
  </div>
</template>

<script>
export default {
  name: "PreloaderEl"
};
</script>

<style lang="scss">
.preloader {
  text-align: center;
  &__img {
    animation: 1s linear preloader infinite;
  }
}
@keyframes preloader {
  0% {
    transform: rotate(0deg);
  }
  100% {
    transform: rotate(360deg);
  }
}
</style>
