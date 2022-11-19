<script setup>
import { ref } from 'vue'
import axios from "axios"

// let movie = document.getElementById("moviesdropdown");
// let chosenMovie = movie.value;
let movies;
let video;
let div;
let movieResponse;
let removeFind;
let trailers;
let movieGenre;
let movieBudget;
let movieRevenue;
let movieRuntime;
let movieHomepage;
let movieTagline;
let movieCollection;
let movieCollectionPoster;
let title;
let poster;
let overview;
let releaseDate;
let genre;
let revenue;
let budget;
let runtime;
let tagline;
let homepage;
let ranking;
let collection;
let collectionPoster;
let trailer;

async function movieOutput() {
  if (removeFind) {
      div.remove();
    }

  div = document.createElement("div");
  div.setAttribute("id", "remover");
    document.body.append(div);

  video = await axios.get(`https://api.themoviedb.org/3/movie/${movies}`, {
    params: {
      api_key: "da6aeec5bd0d488feeebd8b57deda080",
      append_to_response: "videos",
      include_adult: false,
    },
  });
  trailers = video.data.videos.results.filter(
    (trailer) => trailer.type === "Trailer"
  );

  movieGenre = video.data.genres[0].name;
  movieBudget = video.data.budget;
  movieRevenue = video.data.revenue;
  movieRuntime = video.data.runtime;
  movieHomepage = video.data.homepage;
  movieTagline = video.data.tagline;

  title = document.createElement("h1");
title.setAttribute("id", "movie-title");

  poster = document.createElement("img");
poster.setAttribute("id", "movie-poster");
  overview = document.createElement("p");
overview.setAttribute("id", "movie-overview");
  releaseDate = document.createElement("p");
releaseDate.setAttribute("id", "movie-release-date");
  genre = document.createElement("p");
genre.setAttribute("id", "movie-genre");
  revenue = document.createElement("p");
revenue.setAttribute("id", "movie-revenue");
  budget = document.createElement("p");
budget.setAttribute("id", "movie-budget");
  runtime = document.createElement("p");
runtime.setAttribute("id", "movie-runtime");
  tagline = document.createElement("h2");
tagline.setAttribute("id", "movie-tagline");
  homepage = document.createElement("a");
homepage.setAttribute("id", "movie-homepage");
homepage.setAttribute("target", "_blank");
  ranking = document.createElement("p");
ranking.setAttribute("id", "movie-ranking");
  trailer = document.createElement("iframe");
trailer.setAttribute("id", "movie-trailer");

  title.innerHTML = `${video.data.title} `;
  poster.src = `https://image.tmdb.org/t/p/w500${video.data.poster_path}`;
  overview.innerHTML = `Overview: ${video.data.overview}`;
  releaseDate.innerHTML = `Release Date: ${video.data.release_date}`;
  genre.innerHTML = `Genre: ${movieGenre}`;
  revenue.innerHTML = `Revenue: $${movieRevenue}`;
  budget.innerHTML = `Budget: $${movieBudget}`;
  runtime.innerHTML = `Movie Runtime: ${movieRuntime} minutes`;
  tagline.innerHTML = `${movieTagline}`;
  homepage.href = `${movieHomepage}`;
  homepage.innerHTML = "Movie Homepage";
  ranking.innerHTML = `Ranking: ${video.data.vote_average}/10`;

  trailer.src = `https://www.youtube.com/embed/${trailers[0].key}`;

  div.append(title);
  div.append(poster);
  div.append(overview);
  div.append(releaseDate);
  div.append(genre);
  div.append(revenue);
  div.append(budget);
  div.append(runtime);
  div.append(tagline);
  div.append(homepage);
  div.append(ranking);
  if (video.data.belongs_to_collection) {
    movieCollection = video.data.belongs_to_collection.name;
    movieCollectionPoster = video.data.belongs_to_collection.poster_path;

    collection = document.createElement("p");
  collection.setAttribute("id", "movie-collection");
    collectionPoster = document.createElement("img");
  collectionPoster.setAttribute("id", "movie-collection-poster");
    collection.innerHTML = `${movieCollection}`;
    collectionPoster.src = `https://image.tmdb.org/t/p/w500${movieCollectionPoster}`;
    div.append(collection);
    div.append(collectionPoster);
  }
  div.append(trailer);

  removeFind = document.getElementById("remover");
}

// const get = document.getElementById("get");
  // get.addEventListener("click", async () => {
  //   let chosenMovie = movie.value;

  //   if (removeFind) {
  //     div.remove();
  //   }

  //   div = document.createElement("div");
  //   div.setAttribute("id", "remover");
  //   document.body.append(div);

  // movieOutput(chosenMovie);  //should eb 4 lines up

  // pagetitle.setAttribute("class", "animate");

  // });

</script>

<template>
  <!-- use something like this to style the text, to have it out 
  in the template so that it can be styled in the <style> below. Having it will also mean
  the code is cleaned up to be vue. and that the refs have everything. 
  UNDerstand the goal --> 

  <!-- <div id="movieProfile">
        <img id="poster" src="" ref="poster">
        <h1 id="title" ref="title"></h1>
        <h3 id="tagline" ref="tagline"></h3>
        <h3 id="status" ref="status"></h3>
        <p id="popularity" ref="popularity"></p>
        <p id="voteAverage" ref="voteAverage"></p>
        <p id="voteCount" ref="voteCount"></p>
        <p id="budget" ref="budget"></p>
        <p id="overview" ref="overview"></p>

    </div> -->


  <h1 id="pagetitle">Movie Finder</h1>
    <p id="intro">See the drop down below to select a movie</p>
    <form>
      <label id="moviesdropdownlabel" for="moviesdropdown">Choose a movie: </label> 
      <select id="moviesdropdown" v-model="movies">
        <option value="424139">Halloween</option>
        <option value="354912">Coco</option>
        <option value="458156">
          John Wick: Chapter 3 - Parabellum
        </option>
        <option value="146233">Prisoners</option>
        <option value="399566">Godzilla vs. Kong</option>
        <option value="72190">World War Z</option>
        <option value="1422">The Departed</option>
        <option value="12405">Slumdog Millionaire</option>
        <option value="27205">Inception</option>
        <option value="396371">Molly's Game</option>
      </select>
    </form>
    <div class="alignbutton">
      <button id="get" @click="movieOutput()">Get Movie!</button>
    </div>
</template>

<style scoped>
* {
  font-family: "Berkshire Swash", cursive;
}

body {
  background-color: #400000;
  margin: 0px 0px 0px 0px;
}

/* #topdiv {
  background-color: #C9A9A6;
  width:100%;
  aspect-ratio: 10/1;
} */

#pagetitle {
  font-family: "Rye", cursive;
  font-size: 80px;
  text-align: center;
  text-decoration: underline;
  color: #14bebb;
}

#intro {
  margin-left: 5px;
  font-family: "Berkshire Swash", cursive;
  font-size: 45px;
  color: #f37114;
}

#moviesdropdownlabel {
  font-family: "Berkshire Swash", cursive;
  font-size: 70px;
  margin-left: 5px;
  color:#14bebb;
}

#moviesdropdown {
  color: #23395d;
  background-color: #dcecfa;
  width: 800px;
  aspect-ratio: 8/0.6;
  font-size: 40px;
  font-family: "Berkshire Swash", cursive;
}

#moviesdropdown:hover {
  transform: scale(1.047);
  transition-duration: 1s;
}

.alignbutton {
  text-align: center;
}

#get {
  width: 400px;
  height: 100px;
  font-size: 70px;
  color: #29e7d7;
  background-color: #9429f2;
}

#get:hover {
  background-color: #ecd32b;
  color: rgb(22, 22, 144);
  transform: scale(1.5);
  transition-duration: 2s;
}

#remover {
  height: 2000px;
  display: grid;
  border: 6px solid rgb(108, 10, 10);
  background-color: #14bebb;
  margin: 20px 20px 20px 20px;
  grid-template-rows: 100px 30px 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr 1fr;
  grid-template-columns: repeat(4, 1fr);
  justify-items: center;
}

#movie-title {
  grid-column: 2/5;
  text-align: center;
  color: #1c2e4a;
  /* text-shadow: 3px 3px #FE828C; */
  text-shadow: 3px 3px #daab0f;
  font-size: 3.5rem;
  line-height: 1.02;
  align-self: center;
  font-family: "Bungee Inline", cursive;
  /* font-family: 'Fredericka the Great', cursive; */
  /* font-family: 'Metamorphous', cursive; */
}
#movie-poster {
  grid-row: 2/5;
  aspect-ratio: 1/3;
  border: 4px solid rgb(135, 14, 14);
  margin-left: 2px;
  /* put a margin on the poster */
}

#movie-poster:hover {
  transform: skew(-5deg, -5deg);
  transition-duration: 2s;
}

#movie-tagline {
  grid-row: 2/3;
  grid-column: 2/5;
  align-self: center;
}

#movie-overview {
  grid-row: 3/4;
  grid-column: 2/5;
  font-size: 40px;
  align-self: center;
  margin: 20px 20px 0px 20px;
}

#movie-release-date {
  grid-column: 2/3;
  grid-row: 4/5;
  align-self: center;
  font-size: 1.5rem;
  color: #22427a;
}
#movie-genre {
  grid-column: 3/4;
  grid-row: 4/5;
  align-self: center;
  font-size: 1.5rem;
  color: #22427a;
}
#movie-runtime {
  grid-column: 4/5;
  grid-row: 4/5;
  align-self: center;
  font-size: 1.5rem;
  color: #22427a;
}
#movie-revenue {
  grid-column: 2/3;
  grid-row: 5/6;
  align-self: center;
  font-size: 1.5rem;
  color: #22427a;
}
#movie-budget {
  grid-column: 3/4;
  grid-row: 5/6;
  align-self: center;
  font-size: 1.5rem;
  color: #22427a;
}
#movie-homepage {
  grid-column: 3/4;
  grid-row: 6/7;
  font-size: 2rem;
  align-self: end;
}
#movie-ranking {
  grid-column: 4/5;
  grid-row: 5/6;
  align-self: center;
  font-size: 1.5rem;
  color: #22427a;
}
#movie-trailer {
  grid-column: 1/5;
  grid-row: 8/9;
  aspect-ratio: 3/1.87;
  width: 50%;
  margin: 5%;
}

#movie-collection {
  grid-column: 2/3;
  grid-row: 9/10;
  justify-self: end;
align-self: center; 
 font-size: 1.8rem;
 margin-right: 5px;
}

#movie-collection-poster {
  grid-column: 3/4;
  grid-row: 9/10;
  justify-self: start;
  aspect-ratio: 2/3;
  width:30%;
}

@keyframes ani {
  0%   {color:red; }
  15%  {color:rgb(222, 128, 22); }

   /* 30%  {color:blue; } */
  45%  {color:green;}
  50% {color:black}
  /* 60%{color:red; } */
  /* 75%  {color:violet; } */
  80% {color: aqua;}
  /* 100% {color:bisque;}  */

}

.animate {
 animation-name: ani;
 animation-duration: 2s;
 animation-iteration-count: infinite;
}

#hiddenone {
  position: absolute;
  top:1px;
  left: 2px;
  background-color: #400000;
}
</style>
