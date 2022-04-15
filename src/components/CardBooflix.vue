<template>
  <li class="card">
    <img class="first-img" v-if="poster_path==null" :src="setPoster(poster_path)" :alt="original_title">
    <img class="first-img" v-else :src="`${imgUrl}${poster_path}`" :alt="original_title">
    <div class="hoverlay">
      <div>
        <strong>Titolo: </strong><small>{{ title }}</small>
      </div>
      <div v-show="title != original_title">
          <strong>Titolo originale: </strong><small>{{ original_title }}</small>
      </div>
      <div>
        <strong>Lingua: </strong>
        <lang-flag :iso="original_language" :squared="false" />
      </div>
      <div v-show="idGenere.length !== 0">
        <strong>Genere: </strong>
        <small
          v-show="idGenere.length !== 0 && type === 'movie'"
          v-for="(genere, i) in arrfiltergenreFilm"
          :key="genere">{{ genere }}<span v-show="i !== arrfiltergenreFilm.length - 1">, </span>
        </small>
        <small
          v-show="idGenere.length !== 0 && type === 'serie'"
          v-for="(genere, i) in arrfiltergenreSerie"
          :key="genere">{{ genere }}<span v-show="i !== arrfiltergenreSerie.length - 1">, </span>
        </small>
      </div>
      <div>
        <strong>Voto: </strong>
        <small :class="i < voteMovie(vote_average) ? 'stars' : ''"
          v-for="(element, i) in 5"
          :key="i">
          &starf;
        </small>
      </div>
      <button v-show="actors.length != 0"  @click="actorList = !actorList">Vedi Cast</button>
      <div>
        <strong>Trama: </strong>
        <small v-if="overview == ''">n/d</small>
        <small v-else>{{ overview }}</small>
      </div>
    </div>
    <div v-show="actorList && actors.length != 0" @mouseover="actorList = true" @mouseout="actorList = false" class="hoverlay-actors">
      <div>
        <strong>Lista attori: </strong>
      </div>
      <ul>
        <li v-for="actor in actors" :key="actor.id">
          <img v-if="actor.profile_path==null" :src="setPoster(actor.profile_path)" :alt="actor.name">
          <img v-else :src="`${imgProfileUrl}${actor.profile_path}`" :alt="actor.name">
          {{ actor.name }}
          </li>
      </ul>
    </div>
  </li>
</template>

<script>
import axios from "axios";

import LangFlag from 'vue-lang-code-flags';

import logo from '../assets/img/logo-small.png'
import profile from '../assets/img/profile.png'

export default {
  name: 'CardBooflix',
  data () {
    return {
      imgUrl: 'https://image.tmdb.org/t/p/w342',
      imgProfileUrl: 'https://image.tmdb.org/t/p/w45',
      actors: [],
      actorList: false,
      arrfiltergenreFilm: [],
      arrfiltergenreSerie: []
    }
  },
  components: {
    LangFlag,
  },
  props: {
    poster_path: String,
    original_title: String,
    title: String,
    original_language: String,
    vote_average: Number,
    overview: String,
    id: Number,
    idGenere: Array,
    type: String,
    arrGenresFilm: Array,
    arrGenresSerie: Array
  },
  methods: {
    setPoster(x) {
      x== null
      return logo
    },
    setProfile(x) {
      x== null
      return profile
    },
    voteMovie(x) {
      return Math.ceil(x / 2);
    },
    getFilterFilm() {
      if (this.type === "movie" && this.idGenere.length !== 0) {
        for (let i = 0; i < this.arrGenresFilm.length; i++ ) {
          for (let y = 0; y < this.idGenere.length; y++ ) {
            if (this.arrGenresFilm[i].id == this.idGenere[y]) {
              this.arrfiltergenreFilm.push(this.arrGenresFilm[i].name)
            }
          }
        }
      }
    },
    getFilterSerie() {
      if (this.type === "serie" && this.idGenere.length !== 0){
        for (let i = 0; i < this.arrGenresSerie.length; i++ ) {
          for (let y = 0; y < this.idGenere.length; y++ ) {
            if (this.arrGenresSerie[i].id == this.idGenere[y]) {
              this.arrfiltergenreSerie.push(this.arrGenresSerie[i].name)
            }
          }
        }
      }
    }
  },
  created() {
    this.getFilterFilm()
    this.getFilterSerie()
  },
  mounted() {
    if (this.type === "movie") {
			axios.get(`https://api.themoviedb.org/3/movie/${this.id}/credits?api_key=78ffb2ad086d0f2d032bbd3a14b711e0`)
			.then(res => {
				this.actors = res.data.cast.splice(0, 5)
			})
		}else if (this.type === "serie") {
			axios.get(`https://api.themoviedb.org/3/tv/${this.id}/credits?api_key=78ffb2ad086d0f2d032bbd3a14b711e0`)
			.then(res => {
				this.actors = res.data.cast.splice(0, 5)
			})
		}
  }
}
</script>

<style scoped lang="scss">
@import '../assets/styles/partial/variables';
li.card {
    flex: 0 0 342px;
    position: relative;
    margin-bottom: .5rem;
    overflow: hidden;
    cursor: pointer;
    &:hover img.first-img{
      opacity: 0;
      transform: rotateY(180deg);
    }
    &:hover .hoverlay {
      opacity: 1;
      transform: rotateY(0deg);
    }
    img.first-img {
      width: 100%;
      height: 100%;
      object-fit: cover;
      display: block;
      opacity: 1;
      border: 1px solid #fff;
      transform: rotateY(0deg);
      transition: transform 1s ease-in-out, opacity .5s step-end;
    }
    .hoverlay::-webkit-scrollbar {
        background-color: black;
        width: 15px;
        overflow: hidden;
    }
    .hoverlay::-webkit-scrollbar-thumb {
        background: $title-color;
        border-radius: 10px;
        -webkit-border-radius: 12px;
        -webkit-box-shadow: inset 5px 5px 6px rgba(0,0,0,.6);
    }
    .hoverlay {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      align-items: center;
      padding: 1rem;
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      opacity: 0;
      overflow: auto;
      scrollbar-color: $title-color blue;
      border: 1px solid #fff;
      color: #fff;
      background-color: #000;
      transform: rotateY(180deg);
      transition: transform 1s ease-in-out, opacity .5s step-end;
      .flag-icon {
        width: 50px;
        line-height: 40px;
      }
      .stars {
        color: $title-color;
      }
      button {
        padding: .4rem .6rem;
        background-color: $title-color;
        color: #fff;
        border: none;
        border-radius: 5px;
        cursor: pointer;
        &:hover {
          box-shadow: inset 0 0 5px 3px rgba($color: #000, $alpha: .3);
        }
      }
    }
    .hoverlay-actors {
      display: flex;
      flex-direction: column;
      gap: 1rem;
      align-items: center;
      padding: 1rem;
      position: absolute;
      top: 0;
      right: 0;
      bottom: 0;
      left: 0;
      opacity: 1;
      overflow: auto;
      border: 1px solid #fff;
      color: #fff;
      background-color: #000;
      img {
        border: 1px solid #fff;
        border-radius: 5px;
        width: 45px;
        height: 68px;
      }
      ul {
        list-style: none;
        li {
          display: flex;
          align-items: center;
          gap: 2rem;
          margin-bottom: 1rem
        }
      }
    }
  }
</style>
