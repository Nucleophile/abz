<template>
  <div class="userCard">
    <img class="userCard__img" :src="user.photo" :alt="user.name + ' Photo'" />
    <p class="userCard__name" :title="user.name">{{ user.name }}</p>
    <p :title="user.position">{{ user.position }}</p>
    <p :title="user.email"><a :href="'mailto:' + user.email" class="userCard__email-link">{{ user.email }}</a></p>
    <p :title="user.phone">{{ user.phone }}</p>
  </div>
</template>

<script>
export default {
  name: "UserCard",
  props: {
    user: Object,
  },
};
</script>

<style lang="scss">
.userCard {
  &__img {
    max-width: rem(70);
    border-radius: rem(70);
    margin-bottom: rem(20);
  }
  &__name {
    margin-bottom: rem(20);
  }
  &__email-link {
    color: inherit;
  }
  p {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
  }
}
</style>
