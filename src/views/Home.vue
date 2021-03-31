<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-for="place in places" :key="place.id">
      {{ place.name }}
      <br />
      {{ place.address }}
      <hr />
    </div>
  </div>
</template>

<style></style>

<script>
import axios from "axios";
export default {
  data: function () {
    return {
      message: "Vermont Places > Everywhere Else",
      places: [],
    };
  },
  created: function () {
    this.indexPlaces();
  },
  methods: {
    indexPlaces: function () {
      axios.get("/api/places").then((response) => {
        console.log("places index working", response);
        this.places = response.data;
      });
    },
  },
};
</script>
