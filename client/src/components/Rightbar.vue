<!-- Renders ellipsis menu button and right sidebar menu -->

<template>
  <section>
    <!-- Right sidebar menu begins -->
    <b-sidebar
      type="is-light"
      :fullheight="fullheight"
      :fullwidth="fullwidth"
      :overlay="overlay"
      :right="right"
      v-model="open"
      :gardens="gardens"
    >
      <div class="p-1">
        <b-menu>
          <b-menu-list label="Garden Menu">
            <b-menu-item
              v-for="garden in gardens"
              :key="garden.id"
              :label="garden.name"
              @click="goToGarden(garden.id)"
            ></b-menu-item>
            <router-link to="/addgarden">
              <b-button type="is-success is-light">Add a Garden</b-button>
            </router-link>
            <b-menu-item
              label="Delete a Garden"
              type="is-danger"
              @click="toggleGardens"
            >
              <b-button
                type="is-danger d-button"
                v-for="garden in gardens"
                :key="garden.id"
                :label="garden.name"
                @click="deleteGarden(garden.id)"
                class="d-button"
              >
              </b-button>
            </b-menu-item>
          </b-menu-list>
        </b-menu>
      </div>
    </b-sidebar>
    <!-- Right sidebar menu ends -->
    <!-- Ellipsis menu button begins -->
    <b-button class="nav-button" @click="open = true"
      ><b-icon icon="dots-vertical"></b-icon
    ></b-button>
    <!-- Ellipsis menu button ends -->
  </section>
</template>

<script>
import axios from "axios";
import router from "../router";
import { DialogProgrammatic as Dialog } from "buefy";

export default {
  name: "rightbar",
  props: ["user", "swipeOpen", "gardens"],
  data() {
    return {
      open: false,
      overlay: true,
      fullheight: true,
      fullwidth: false,
      right: true,
      deleting: false,
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

  methods: {
    goToGarden(id) {
      this.$router
        .push({
          path: "/garden",
          query: { id },
        })
        .catch(() => {
          this.$log.info("same route");
        });
    },
    toggleGardens() {
      this.deleting = !this.deleting;
    },
    deleteGarden(id) {
      if (confirm("Are you sure you want to delete?")) {
        axios({
          method: "DELETE",
          url: "garden/deletegarden",
          data: { id: id },
        })
          .then((data) => {
            let newGardens = [...this.gardens];
            newGardens = newGardens.filter((g) => g.id !== id);
            console.log(newGardens);
            this.$emit("edit:gardens", newGardens);
            router.push({
              name: "profile",
            });
            this.$buefy.toast.open({
              message: "Garden Deleted",
              type: "is-success",
              duration: 1000,
            });
          })
          .catch((err) => console.log(err));
      }
    },
  },
};
</script>
<style lang="sass">
.d-button
  margin: 3px
</style>
