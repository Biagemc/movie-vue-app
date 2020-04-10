<template>
  <div class="home">
    <h1>{{ message }}</h1>
    <div>
      <p>
        <input v-model="newActorFirstName" />
      </p>
      <p>
        <input v-model="newActorLastName" />
      </p>
      <p>
        <input v-model="newActorAge" />
      </p>
      <p>
        <input v-model="newActorKnownFor" />
      </p>
      <p>
        <input v-model="newActorMovieId" />
      </p>
      <p>
        <button v-on:click="addActor()">Create an actor</button>
      </p>
    </div>
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
      newActorFirstName: "",
      newActorLastName: "",
      newActorAge: "",
      newActorKnownFor: "",
      newActorMovieId: "",
    };
  },
  created: function() {
    axios.get("/api/actors").then(response => {
      console.log(response.data);
      this.actors = response.data;
    });
  },
  methods: {
    addActor: function() {
      let params = {
        first_name: this.newActorFirstName,
        last_name: this.newActorLastName,
        age: this.newActorAge,
        known_for: this.newActorKnownFor,
        movie_id: this.newActorMovieId,
      };
      axios.post("/api/actors", params).then(response => {
        this.actors.push(params);
        this.newActorFirstName = "";
        this.newActorLastName = "";
        this.newActorAge = "";
        this.newActorKnownFor = "";
        this.newActorMovieId = "";
      });
    },
  },
};
</script>