<script setup>
import { ref } from "vue";
import axios from "axios";

let movies =  ref(null);
let video;
let removeIf = ref(false);
let trailers = ref();
let title = ref();
let poster = ref();
let overview = ref();
let releaseDate = ref();
let genre = ref();
let revenue = ref();
let budget = ref();
let runtime = ref();
let tagline = ref();
let homepage = ref();
let ranking = ref();
let collection = ref();
let collectionPoster = ref();
let trailer = ref();
let collectionTF = ref();
let titleAnimation = ref();

async function movieOutput() {
  if (movies.value) {
    removeIf.value = true;
  }
  if (removeIf.value) {
    titleAnimation.value = "animate";
  }
  collectionTF.value = true;

  video = await axios.get(`https://api.themoviedb.org/3/movie/${movies.value}`, {
    params: {
      api_key: "da6aeec5bd0d488feeebd8b57deda080",
      append_to_response: "videos",
      include_adult: false,
    },
  });
  trailers = video.data.videos.results.filter(
    (trailer) => trailer.type === "Trailer"
  );

  title.value = `${video.data.title} `;
  poster.value = `https://image.tmdb.org/t/p/w500${video.data.poster_path}`;
  overview.value = `Overview: ${video.data.overview}`;
  releaseDate.value = `Release Date: ${video.data.release_date}`;
  genre.value = `Genre: ${video.data.genres[0].name}`;
  revenue.value = `Revenue: $${video.data.revenue}`;
  budget.value = `Budget: $${video.data.budget}`;
  runtime.value = `Movie Runtime: ${video.data.runtime} minutes`;
  tagline.value = `${video.data.tagline}`;
  homepage.value = `${video.data.homepage}`;
  ranking.value = `Ranking: ${video.data.vote_average}/10`;
  trailer.value = `https://www.youtube.com/embed/${trailers[0].key}`;

  if (video.data.belongs_to_collection) {
    collection.value = `${video.data.belongs_to_collection.name}`;
    collectionPoster.value = `https://image.tmdb.org/t/p/w500${video.data.belongs_to_collection.poster_path}`;
  } else {
    collectionTF.value = false;
  }
}
</script>

<template>
  <div id="main-div">
    <h1 id="pagetitle" :class="titleAnimation">Movie Finder</h1>
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
      <button id="get" @click="movieOutput()">Get Movie!</button>
    </div>

    <div id="remover" v-if="removeIf">
      <img id="movie-poster" :src="poster" />
      <p id="movie-title">{{ title }}</p>
      <p id="movie-overview">{{ overview }}</p>
      <p id="movie-release-date">{{ releaseDate }}</p>
      <p id="movie-genre">{{ genre }}</p>
      <p id="movie-revenue">{{ revenue }}</p>
      <p id="movie-budget">{{ budget }}</p>
      <p id="movie-runtime">{{ runtime }}</p>
      <p id="movie-tagline">{{ tagline }}</p>
      <a id="movie-homepage" target="_blank" :href="homepage">Movie Homepage</a>
      <iframe id="movie-trailer" :src="trailer"></iframe>
      <p id="movie-ranking">{{ ranking }}</p>
      <p v-if="collectionTF" id="movie-collection">{{ collection }}</p>
      <img
        v-if="collectionTF"
        id="movie-collection-poster"
        :src="collectionPoster"
      />
    </div>
  </div>
</template>

<style scoped>
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
  margin-top: 2rem;
  margin-bottom: 2.1rem;
}

#get:hover {
  background-color: #ecd32b;
  color: rgb(22, 22, 144);
  transform: scale(1.5);
  transition-duration: 2s;
}

#get:focus {
  border: 2px solid white;
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
  text-shadow: 3px 3px #daab0f;
  font-size: 3.5rem;
  line-height: 1.02;
  align-self: center;
  font-family: "Bungee Inline", cursive;
  font-weight: bold;
}
#movie-poster {
  grid-row: 2/5;
  aspect-ratio: 1/3;
  border: 4px solid rgb(135, 14, 14);
  margin-left: 2px;
}

#movie-poster:hover {
  transform: skew(-5deg, -5deg);
  transition-duration: 2s;
}

#movie-tagline {
  font-weight: bold;
  font-size: 1.5rem;
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
  border: 0px;
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
  45% {
    color: green;
  }
  50% {
    color: black;
  }
  80% {
    color: aqua;
  }
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
