<template>
  <b-navbar>
    <template slot="brand">
      <b-navbar-item tag="router-link" :to="{ path: '/' }">
        Horter
        <img id="brand-image" :src="brandImageUrl" />
      </b-navbar-item>
    </template>
    <template v-if="user.id" slot="start">
      <b-navbar-item tag="router-link" :to="{ path: 'UserProfile' }">
        Profile
      </b-navbar-item>
      <b-navbar-item tag="router-link" :to="{ path: 'plant' }">
        Plantlist
      </b-navbar-item>
      <b-navbar-item tag="router-link" :to="{ path: 'wish' }">
        Wishlist
      </b-navbar-item>
      <b-navbar-item tag="router-link" :to="{ path: 'FriendSearch' }">
        Search Friends
      </b-navbar-item>
      <b-navbar-dropdown label="Gardens">
        <b-navbar-item
          v-for="garden in gardens"
          :key="garden.id"
          tag="router-link"
          :to="{ path: '/garden', query: { id: garden.id } }"
        >
          {{ garden.name }}
        </b-navbar-item>
        <b-navbar-item tag="router-link" :to="{ path: 'addgarden' }">
          Add Garden
        </b-navbar-item>
      </b-navbar-dropdown>
    </template>
    <template slot="end">
      <b-navbar-item v-if="!user.id" tag="a" href="/auth/google">
        Login
      </b-navbar-item>
      <b-navbar-item v-else tag="a" href="/auth/logout"> Logout </b-navbar-item>
    </template>
  </b-navbar>
</template>

<script>
import axios from "axios";

export default {
  name: "DesktopNavigation",
  props: ["user", "gardens"],
  data() {
    return {
      brandImageUrl: require("../assets/horter.png"),
    };
  },
};
</script>

<style lang="sass">
#brand-image
  height: 40px
  margin: 10px
</style>
