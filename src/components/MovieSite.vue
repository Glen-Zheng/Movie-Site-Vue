<script setup>
import { ref } from "vue";
import axios from "axios";

// let movies;
// let div;
// let video ;
// let removeFind;
// let trailers ;
// let title  ;
// let poster ;
// let overview ;
// let releaseDate ;
// let genre ;
// let revenue ;
// let budget;
// let runtime ;
// let tagline ;
// let homepage ;
// let ranking ;
// let collection ;
// let collectionPoster ;
// let trailer ;

let movies;
// let div = ref(null);
let video = ref(null);
// let removeFind = ref(null);
let removeIf = ref(false);
let trailers = ref(null);
let title = ref(null);
let poster = ref(null);
let overview = ref(null);
let releaseDate = ref(null);
let genre = ref(null);
let revenue = ref(null);
let budget = ref(null);
let runtime = ref(null);
let tagline = ref(null);
let homepage = ref(null);
let ranking = ref(null);
let collection = ref(null);
let collectionPoster = ref(null);
let trailer = ref(null);
let collectionTF = ref(false);

async function movieOutput() {

  // div = document.createElement("div");
  // div.setAttribute("id", "remover");
  // document.body.append(div);
  removeIf.value = true;

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

  // title = document.createElement("h1");
  // title.setAttribute("id", "movie-title");
  // poster = document.createElement("img");
  // poster.setAttribute("id", "movie-poster");
  // overview = document.createElement("p");
  // overview.setAttribute("id", "movie-overview");
  // releaseDate = document.createElement("p");
  // releaseDate.setAttribute("id", "movie-release-date");
  // genre = document.createElement("p");
  // genre.setAttribute("id", "movie-genre");
  // revenue = document.createElement("p");
  // revenue.setAttribute("id", "movie-revenue");
  // budget = document.createElement("p");
  // budget.setAttribute("id", "movie-budget");
  // runtime = document.createElement("p");
  // runtime.setAttribute("id", "movie-runtime");
  // tagline = document.createElement("h2");
  // tagline.setAttribute("id", "movie-tagline");
  // homepage = document.createElement("a");
  // homepage.setAttribute("id", "movie-homepage");
  // homepage.setAttribute("target", "_blank");
  // ranking = document.createElement("p");
  // ranking.setAttribute("id", "movie-ranking");
  // trailer = document.createElement("iframe");
  // trailer.setAttribute("id", "movie-trailer");

  title.value.innerHTML = `${video.data.title} `;
  poster.value.src = `https://image.tmdb.org/t/p/w500${video.data.poster_path}`;
  overview.value.innerHTML = `Overview: ${video.data.overview}`;
  releaseDate.value.innerHTML = `Release Date: ${video.data.release_date}`;
  genre.value.innerHTML = `Genre: ${video.data.genres[0].name}`;
  revenue.value.innerHTML = `Revenue: $${video.data.revenue}`;
  budget.value.innerHTML = `Budget: $${video.data.budget}`;
  runtime.value.innerHTML = `Movie Runtime: ${video.data.runtime} minutes`;
  tagline.value.innerHTML = `${video.data.tagline}`;
  homepage.value.href = `${video.data.homepage}`;
  homepage.value.innerHTML = "Movie Homepage";
  ranking.value.innerHTML = `Ranking: ${video.data.vote_average}/10`;
  trailer.value.src = `https://www.youtube.com/embed/${trailers[0].key}`;

  // div.append(title);
  // div.append(poster);
  // div.append(overview);
  // div.append(releaseDate);
  // div.append(genre);
  // div.append(revenue);
  // div.append(budget);
  // div.append(runtime);
  // div.append(tagline);
  // div.append(homepage);
  // div.append(ranking);
  if (video.data.belongs_to_collection) {

    collectionTF.value = true;

    // collection = document.createElement("p");
    // collection.setAttribute("id", "movie-collection");
    // collectionPoster = document.createElement("img");
    // collectionPoster.setAttribute("id", "movie-collection-poster");
    collection.value.innerHTML = `${video.data.belongs_to_collection.name}`;
    collectionPoster.value.src = `https://image.tmdb.org/t/p/w500${video.data.belongs_to_collection.poster_path}`;
    // div.append(collection);
    // div.append(collectionPoster);
  }
  // div.append(trailer);

  // removeFind = document.getElementById("remover");


}
</script>

<template>

  <!-- use something like this to style the text, to have it out 
  in the template so that it can be styled in the <style> below. Having it will also mean
  the code is cleaned up to be vue. and that the refs have everything. 
  UNDerstand the goal -->
  <!--is the solution to use refs ( call it in the attributes) and then it'll update? only when get is pressed the info is updated-->

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

<div id="main-div">


  <h1 id="pagetitle">Movie Finder</h1>
  <p id="intro">See the drop down below to select a movie</p>
  <form>
    <label id="moviesdropdownlabel" for="moviesdropdown"
      >Choose a movie:
    </label>
    <select id="moviesdropdown" v-model="movies">
      <option value="424139">Halloween</option>
      <option value="354912">Coco</option>
      <option value="458156">John Wick: Chapter 3 - Parabellum</option>
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
    <button id="get" @click="movieOutput()" >Get Movie!</button>
  </div>


  <div id="remover" v-if="removeIf">
    <img id = "movie-poster" src =" " ref= "poster"/>
    <p id="movie-title" ref= "title"></p>
    <p id="movie-overview" ref= "overview"></p>
    <p id="movie-release-date" ref= "releaseDate"></p>
    <p id="movie-genre" ref= "genre"></p>
    <p id="movie-revenue" ref= "revenue"></p>
    <p id="movie-budget" ref= "budget"></p>
    <p id="movie-runtime" ref= "runtime" ></p>
    <p id="movie-tagline" ref= "tagline"></p>
    <a id="movie-homepage" target="_blank" href= " " ref= "homepage"></a>
    <iframe id ="movie-trailer" src=" " ref= "trailer"></iframe>
    <p id ="movie-ranking" ref= "ranking"></p>

    <div v-if="collectionTF">
    <p id ="movie-collection" ref="collection"></p>
    <img id="movie-collection-poster" src = " " ref="collectionPoster" />
    </div>
  </div>

</div>

</template>

<style scoped>


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
  color: #14bebb;
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

  overflow: auto;
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
  /* margin:0; */
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
  width: 30%;
}

@keyframes ani {
  0% {
    color: red;
  }
  15% {
    color: rgb(222, 128, 22);
  }

  /* 30%  {color:blue; } */
  45% {
    color: green;
  }
  50% {
    color: black;
  }
  /* 60%{color:red; } */
  /* 75%  {color:violet; } */
  80% {
    color: aqua;
  }
  /* 100% {color:bisque;}  */
}
.animate {
  animation-name: ani;
  animation-duration: 2s;
  animation-iteration-count: infinite;
}

#hiddenone {
  position: absolute;
  top: 1px;
  left: 2px;
  background-color: #400000;
}
</style>
