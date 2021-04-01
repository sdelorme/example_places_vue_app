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
      <br />
      <button v-on:click="showPlace(place)">Click for more info</button>
      <hr />
    </div>
    <dialog id="place-details">
      <form method="dialog">
        <p>
          Name:
          <input v-model="currentPlace.name" />
        </p>
        <p>
          Address:
          <input v-model="currentPlace.address" />
        </p>
        <button v-on:click="updatePlace(currentPlace)">Update Place Info</button>
        <button v-on:click="destroyPlace(currentPlace)">Delete This Place</button>
        <button>Close</button>
      </form>
    </dialog>
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
      currentPlace: {},
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
    showPlace: function (place) {
      this.currentPlace = place;
      document.querySelector("#place-details").showModal();
    },
    updatePlace: function (place) {
      var params = {
        name: this.newPlaceName,
        address: this.newPlaceAddress,
      };
      axios.patch("/api/places/" + place.id, params).then((response) => {
        console.log("updating place", response);
        this.currentPlace = {};
      });
    },
    destroyPlace: function (place) {
      axios.delete("/api/places/" + place.id).then((response) => {
        console.log("destroying place", response);
        var index = this.places.indexOf(place);
        this.places.splice(index, 1);
      });
    },
  },
};
</script>
