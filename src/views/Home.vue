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
      <button v-on:click="showActor(actor)">More Info</button>

      <div v-if="currentActor === actor">
        <p>Age: {{actor.age}}</p>
        <p>Known For: {{actor.known_for}}</p>
        <p>
          <input v-model="actor.first_name" />
        </p>
        <p>
          <input v-model="actor.last_name" />
        </p>
        <p>
          <input v-model="actor.age" />
        </p>
        <p>
          <input v-model="actor.known_for" />
        </p>
        <p>
          <input v-model="actor.movie_id" />
        </p>
        <p>
          <button v-on:click="updateActor(actor)">Update</button>
        </p>
        <p>
          <button v-on:click="deleteActor(actor)">Delete</button>
        </p>
      </div>
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
      currentActor: "",
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
    showActor: function(theActor) {
      this.currentActor = theActor;
    },
    updateActor: function(theActor) {
      let params = {
        first_name: theActor.first_name,
        last_name: theActor.last_name,
        age: theActor.age,
        known_for: theActor.known_for,
        movie_id: theActor.movie_id,
      };
      axios.patch("/api/actors/" + theActor.id, params).then(response => {
        console.log("Updating actors info...");
        this.currentActor = {};
      });
    },
    deleteActor: function(theActor) {
      axios.delete("/api/actors/" + theActor.id).then(response => {
        console.log("Deleting actor info...");
        let index = this.actors.indexOf(theActor);
        this.actors.splice(index, 1);
      });
    },
  },
};
</script>