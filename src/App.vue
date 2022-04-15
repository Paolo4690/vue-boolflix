<template>
  <div id="app">
    <HeaderBooflix
      @resetArray="resetArray"
      @search="searchFilmTv"
      @onlyFilm="onlyFilm"
      @onlySeries="onlySeries"
      @genreTvSelected="genreTvSelected"
      @genreMovieSelected="genreMovieSelected"
      :arrGenresFilm = arrGenresFilm
      :arrGenresSerie = arrGenresSerie
     />
    <MainBooflix
      :movies= arrMovies
      :series = arrSeries
      :pagesMovies = pagesMovies
      :pagesSeries = pagesSeries
      :currentPagesMovies = currentPagesMovies
      :currentPagesSeries = currentPagesSeries
      :arrGenresFilm = arrGenresFilm
      :arrGenresSerie = arrGenresSerie
      :genereMovieSelected = genereMovieSelected
      :genereTvSelected = genereTvSelected
      @changePageMovie ="changePageMovie"
      @changePageSeries ="changePageSeries"
    />
  </div>
</template>

<script>
import HeaderBooflix from './components/HeaderBooflix.vue'
import MainBooflix from './components/MainBooflix.vue'

import axios from "axios";

export default {
  name: 'App',
  components: {
    HeaderBooflix,
    MainBooflix
  },
  data () {
    return {
      url: 'https://api.themoviedb.org/3',
      apiKey: '78ffb2ad086d0f2d032bbd3a14b711e0',
      arrMovies: [],
      arrSeries: [],
      pagesMovies: null,
      pagesSeries: null,
      currentPagesMovies: null,
      currentPagesSeries: null,
      str:'',
      arrGenresFilm: [],
      arrGenresSerie: [],
      genereMovieSelected: '',
      genereTvSelected: ''
    }
  },
  methods: {
    genreMovieSelected(idGenere) {
      this.genereMovieSelected = idGenere
    },
    genreTvSelected(idGenere) {
      this.genereTvSelected = idGenere
    },
    changePageMovie(i) {
      this.currentPagesMovies = i
      axios.get(`${this.url}/search/movie?api_key=${this.apiKey}&language=it-IT&query=${this.str}&page=${i}`)
      .then((response) => {
        this.arrMovies = response.data.results
      })
    },
    changePageSeries(i) {
      this.currentPagesSeries = i
      axios.get(`${this.url}/search/tv?api_key=${this.apiKey}&language=it-IT&query=${this.str}&page=${i}`)
      .then((response) => {
        this.arrSeries = response.data.results
      })
    },
    searchFilmTv(value) {
      this.str= value
      if (value != '') {
        const objParams = {
          api_key: this.apiKey,
          language: 'it-IT',
          query: value
        };

        this.callAxios('movie', objParams)
        this.callAxios('tv', objParams)
      }
    },
    onlyFilm(value) {
      this.str= value
      this.arrSeries= []
      const objParams = {
        api_key: this.apiKey,
        language: 'it-IT',
        query: value
      };

      this.callAxios('movie', objParams)
    },
    onlySeries(value) {
      this.str= value
      this.arrMovies= []
      const objParams = {
        api_key: this.apiKey,
        language: 'it-IT',
        query: value
      };

      this.callAxios('tv', objParams)
    },
    callAxios(type, objParams) {
      axios(`${this.url}/search/${type}`, {
          params: objParams
        })
          .then((response) => {
            if (type == 'movie') {
              this.arrMovies = response.data.results
              this.pagesMovies = response.data.total_pages
              this.currentPagesMovies = response.data.page
            } else {
              this.arrSeries = response.data.results
              this.pagesSeries = response.data.total_pages
              this.currentPagesSeries = response.data.page
            }
          });
    },
    resetArray(value) {
      this.arrMovies = value
      this.arrSeries = value
    }
  },
  mounted() {
    axios.get(`https://api.themoviedb.org/3/genre/movie/list?api_key=78ffb2ad086d0f2d032bbd3a14b711e0&language=it-IT`)
			.then(res => {
				this.arrGenresFilm = res.data.genres
			})
      axios.get(`https://api.themoviedb.org/3/genre/tv/list?api_key=78ffb2ad086d0f2d032bbd3a14b711e0&language=it-IT`)
			.then(res => {
				this.arrGenresSerie = res.data.genres
			})
  }
}
</script>

<style lang="scss">
@import './assets/styles/style';

</style>
