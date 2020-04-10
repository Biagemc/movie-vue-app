<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div v-bind:key="actor.id" v-for="actor in actors">
      <p>First Name: {{actor.first_name}}</p>
      <p>Last Name: {{actor.last_name}}</p>
      <p>Age: {{actor.age}}</p>
      <p>Known For: {{actor.known_for}}</p>
      <hr />
    </div>
  </div>
</template>

<style>
</style>

<script>
import axios from "axios";

export default {
  data: function() {
    return {
      message: "Movie App",
      actors: [],
    };
  },
  created: function() {
    axios.get("/api/actors").then(response => {
      console.log(response.data);
      this.actors = response.data;
    });
  },
  methods: {},
};
</script>