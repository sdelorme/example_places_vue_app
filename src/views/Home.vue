<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <p>
      Place Name:
      <input v-model="newPlaceName" />
    </p>
    <p>
      Place Address:
      <input v-model="newPlaceAddress" />
    </p>
    <button v-on:click="createPlace()">Add Place To List</button>
    <hr />
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
      newPlaceName: "",
      newPlaceAddress: "",
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
    createPlace: function () {
      var params = {
        name: this.newPlaceName,
        address: this.newPlaceAddress,
      };
      axios.post("/api/places", params).then((response) => {
        console.log("creating place", response);
        this.places.push(response.data);
        this.newPlaceName = "";
        this.newPlaceAddress = "";
      });
    },
  },
};
</script>
