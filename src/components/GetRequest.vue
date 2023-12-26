<template>
  <article class="get-request" id="users">
    <div class="container">
      <h1>Working with GET request</h1>
      <ul class="users-list" v-if="users.length">
        <li class="users-list__item" v-for="user in users" :key="user.id">
          <UserCard :user="user" />
        </li>
      </ul>
      <p class="error-msg" v-if="loadingError">
        Can't load users list. Please, try again later
      </p>
      <PreloaderEl v-if="loading" />
      <button class="btn btn--default" @click="loadUsers" v-if="showMoreShown">
        Show more
      </button>
    </div>
  </article>
</template>

<script>
import { ref, computed, watch, toRefs } from "vue";
import UserCard from "./UserCard.vue";
import PreloaderEl from "./PreloaderEl.vue";

export default {
  name: "GetRequest",
  props: {
    usersReloadFlag: Number,
  },
  setup(props) {
    const users = ref([]);
    let loading = ref(true);
    let loadingError = ref(false);
    const defaultLoadingUrl =
      "https://frontend-test-assignment-api.abz.agency/api/v1/users?count=6";
    let loadingUrl = ref(defaultLoadingUrl);
    const showMoreShown = computed(() => (loadingUrl.value ? true : false));
    const { usersReloadFlag } = toRefs(props);

    loadUsers();

    watch(usersReloadFlag, () => {
      loadingUrl.value = defaultLoadingUrl;
      users.value = [];
      loadUsers();
    });

    function loadUsers() {
      loading.value = true;
      fetch(loadingUrl.value)
        .finally(() => (loading.value = false))
        .then((res) => res.json())
        .then((resJSON) => {
          users.value = users.value.concat(resJSON.users);
          loadingUrl.value = resJSON.links.next_url;
        })
        .catch(() => {
          loadingError.value = true;
        });
    }

    return { users, loading, loadingError, loadUsers, showMoreShown };
  },
  components: {
    PreloaderEl,
    UserCard,
  },
};
</script>

<style lang="scss">
.get-request {
  text-align: center;
}
.users-list {
  display: flex;
  flex-wrap: wrap;
  gap: rem(20);
  margin-bottom: rem(50);
  &__item {
    width: 100%;
  }
  @include media(from_tablet) {
    gap: $user-list-tablet-gap;
    &__item {
      width: math.div($tablet-content - $user-list-tablet-gap, 2);
    }
  }
  @include media(from_laptop) {
    gap: $user-list-laptop-gap;
    &__item {
      width: math.div($laptop-content - $user-list-laptop-gap * 2, 3);
    }
  }
  @include media(desktop) {
    gap: $user-list-desktop-gap;
    &__item {
      width: math.div($desktop-content - $user-list-desktop-gap * 2, 3);
    }
  }
  &__item {
    padding: rem(20);
    border-radius: rem(16);
    background: #fff;
    flex-grow: 1;
  }
}
</style>
