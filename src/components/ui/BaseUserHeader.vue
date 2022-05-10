<template>
  <div class="userHeader">
    <div class="userHeader__photo">
      <img :src="user.photo">
    </div>
    <div class="userHeader__wrap">
      <h3 class="userHeader__name">
        <router-link to="/">{{ user.name }}</router-link>
      </h3>
      <span class="userHeader__time">2022/1/10 12:00</span>
    </div>
  </div>
</template>

<script>
export default {
  props: ['user']
}
</script>
