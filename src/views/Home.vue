<template>
  <div class="home">
    <h1>{{ message }}</h1>

    Title: <input type="text" v-model="newMovieTitle" /><br />
    Year: <input type="text" v-model="newMovieYear" /><br />
    Plot: <input type="text" v-model="newMoviePlot" /><br />
    Director: <input type="text" v-model="newMovieDirector" /><br />

    <button v-on:click="createMovie()">Create Movie</button>
    <div v-for="movie in movies" v-bind:key="movie.id">
      <h1>{{ movie.title }}</h1>
      Release Year: {{ movie.year }} <br />
      Plot: {{ movie.plot }} <br />
      Director: {{ movie.director }} <br />
      Genres: {{ movie.genres }} <br />
    </div>
  </div>
</template>
<style></style>

<script>
import axios from "axios";
export default {
  data: function() {
    return {
      message: "Movie List!",
      movies: [],
      newMovieTitle: "",
      newMovieYear: "",
      newMoviePlot: "",
      newMovieDirector: "",
      newMovieEnglish: ""
    };
  },
  created: function() {
    this.indexMovies();
  },
  methods: {
    indexMovies: function() {
      axios.get("/api/movies/").then(response => {
        this.movies = response.data;
      });
    },
    createMovie: function() {
      var params = {
        title: this.newMovieTitle,
        year: this.newMovieYear,
        plot: this.newMoviePlot,
        director: this.newMovieDirector,
        english: true
      };
      axios
        .post("/api/movies/", params)
        .then(response => {
          console.log("Sanity Test");
          this.movies.push(response.data);
        })
        .catch(error => {
          console.log("Sanity Test, Error-style");
          console.log(error.response.data.errors);
        });
    }
  }
};
</script>
