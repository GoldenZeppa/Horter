<!-- Renders hamburger menu button and left sidebar menu -->

<template>
  <section>
    <!-- Left sidebar menu begins -->
    <b-sidebar
      type="is-light"
      :fullheight="fullheight"
      :fullwidth="fullwidth"
      :overlay="overlay"
      :right="right"
      v-model="open"
    >
      <div class="p-1">
        <b-image
          :src="user.s3_id"
          alt="A random image"
          ratio="4by4"
          :rounded="rounded"
        ></b-image>
        <b-menu>
          <b-menu-list :label="user.username"></b-menu-list>
          <b-menu-list label="Menu">
            <b-menu-item
              icon="home"
              @click="$router.push('/')"
              label="Home"
            ></b-menu-item>
            <b-menu-item
              icon="account"
              label="Profile"
              @click="$router.push('UserProfile')"
            ></b-menu-item>
            <b-menu-item
              icon="sprout"
              label="Plant List"
              @click="$router.push('plant')"
            ></b-menu-item>
            <b-menu-item
              icon="sprout-outline"
              label="Wish List"
              @click="$router.push('wish')"
            ></b-menu-item>
            <b-menu-item
              icon="account-supervisor"
              label="Search Friends"
              @click="$router.push('FriendSearch')"
            ></b-menu-item>
          </b-menu-list>
          <b-menu-list label="Actions">
            <a href="/auth/logout">
              <b-menu-item icon="logout" label="Logout"></b-menu-item>
            </a>
          </b-menu-list>
        </b-menu>
      </div>
    </b-sidebar>
    <!-- Left sidebar menu ends -->
    <!-- Hamburger menu button begins -->
    <b-button class="nav-button" @click="open = true"
      ><b-icon icon="menu"></b-icon
    ></b-button>
    <!-- Hamburger menu button ends -->
  </section>
</template>

<script>
export default {
  name: "sidebar",
  props: ["user", "swipeOpen"],
  data() {
    return {
      open: false,
      overlay: true,
      fullheight: true,
      fullwidth: false,
      right: false,
      rounded: true,
    };
  },
  watch: {
    swipeOpen: function () {
      this.open = this.swipeOpen;
    },
    open: function () {
      if (!this.open) {
        this.$emit("close-sidebars");
      }
    },
  },
};
</script>
