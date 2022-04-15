<template>
  <div class="main">
    <div class="container">
      <div v-show="movies.length == 0 && series.length == 0">
        <h1 class="booflix">Benvenuto su Booflix!</h1>
        <p>Effettua una ricerca per cominciare.</p>
      </div>
      <div class="position-page" v-show="movies.length != 0">
        <div class="cont-page" v-show="pagesMovies > 1">
          <div
            v-for="(pages, i) in setPage(pagesMovies)"
            :key="'movie page' + i "
            :class="i + 1  === currentPagesMovies  ? 'active' : ''"
            class="square-page"
            @click="$emit('changePageMovie', i + 1)"
          >
            {{ i + 1 }}
          </div>
        </div>
        <h1>Lista Film</h1>
        <ul class="cards">
          <CardBooflix
            v-for="movie in arrFilter(movies, genereMovieSelected)"
            :key= movie.id
            :poster_path= movie.poster_path
            :original_title= movie.original_title
            :title= movie.title
            :original_language= movie.original_language
            :vote_average = movie.vote_average
            :overview = movie.overview
            :id= movie.id
            :idGenere = movie.genre_ids
            :arrGenresFilm = arrGenresFilm
            type = 'movie'
          />
        </ul>
      </div>
      <div class="position-page" v-if="series.length != 0">
        <div class="cont-page" v-show="pagesSeries > 1">
          <div
            v-for="(pages, i) in setPage(pagesSeries)"
            :key="'serie page' + i "
            :class="i + 1  === currentPagesSeries  ? 'active' : ''"
            class="square-page"
            @click="$emit('changePageSeries', i + 1)"
          >
            {{ i + 1 }}
          </div>
        </div>
        <h1>Lista Serie TV</h1>
        <ul class="cards">
          <CardBooflix
            v-for="serie in arrFilter(series, genereTvSelected)"
            :key= serie.id
            :poster_path= serie.poster_path
            :original_title= serie.original_name
            :title= serie.name
            :original_language= serie.original_language
            :vote_average = serie.vote_average
            :overview = serie.overview
            :id= serie.id
            :idGenere = serie.genre_ids
            :arrGenresSerie = arrGenresSerie
            type = 'serie'
          />
        </ul>
      </div>
    </div>
  </div>
</template>

<script>
import CardBooflix from './CardBooflix.vue'

export default {
  name: 'MainBooflix',
  data() {
    return {
      maxPage: 5
    }
  },
  props: {
    movies: Array,
    series: Array,
    pagesMovies: Number,
    pagesSeries: Number,
    currentPagesMovies: Number,
    currentPagesSeries: Number,
    arrGenresFilm: Array,
    arrGenresSerie: Array,
    genereMovieSelected: String,
    genereTvSelected: String
  },
  components: {
    CardBooflix,
  },
  methods: {
    setPage(x) {
      if (x >= 5) {
        return 5
      }else {
        return x
      }
    },
    arrFilter(array, genere) {
      if (genere !== '') {
        return array.filter(ele => ele.genre_ids.includes(parseInt(genere)))
        } else {
        return array
      }

    },
  }
}
</script>

<style scoped lang="scss">
@import '../assets/styles/partial/variables';

.main {
  margin-top: 60px;
  min-height: calc(100vh - 60px);
  background-color: $bg-main;
  .container {
    .position-page {
      position: relative;
      .cont-page {
        position: absolute;
        display: flex;
        gap: 5px;
        right: 0;
        top: 10px;
        .square-page.active {
          background-color: $title-color;
        }
        .square-page {
          width: 30px;
          height: 30px;
          border: 1px solid #fff;
          background-color: #000;
          color: #fff;
          line-height: 30px;
          text-align: center;
          cursor: pointer;
          &:hover {
            background-color: $title-color;
          }
        }
      }
    }
    ul {
      display: flex;
      justify-content: center;
      flex-wrap: wrap;
      gap: 1.3rem;
      list-style: none;
    }
    h1 {
      color: #fff;
      text-align: center;
      padding: 1rem 0;
    }
    h1.booflix {
      color: $title-color;
      filter: drop-shadow(4px 4px 1px #0a0a0a);
      font-size: 40px;
    }
    p {
      color: #fff;
      filter: drop-shadow(4px 4px 1px #0a0a0a);
      font-size: 22px;
      text-align: center;
    }
  }
}

</style>
