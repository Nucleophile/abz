<template>
  <header>
    <div class="container">
      <div class="header__content">
        <img src="../assets/logo.svg" alt="TESTTASK" />
        <ul class="header-actions-list">
          <li><a href="#users" class="btn btn--default">Users</a></li>
          <li><a href="#sign-up" class="btn btn--default">Sign up</a></li>
        </ul>
      </div>
    </div>
  </header>
</template>

<script>
export default {
  name: "HeaderBlock",
};
</script>

<style lang="scss">
.header__content {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: rem(13) 0;
}
.header-actions-list {
    display: flex;
    gap: rem(10);
}
</style>
