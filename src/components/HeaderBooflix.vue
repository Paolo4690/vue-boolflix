<template>
  <div class="header">
    <div @click="$emit('resetArray', [])" class="cont-logo">
      <img src="../assets/img/logo.png" alt="">
    </div>
    <div class="cont-txt">
      <h3 @click="$emit('onlyFilm', 'The Avengers')">Film</h3>
      <h3 @click="$emit('onlySeries', 'dragon')">Serie Tv</h3>
    </div>
    <div class="cont-select">
      <label for="genresFilm">Genere Film</label>
      <select @change="$emit('genreMovieSelected', $event.target.value)" name="genresFilm" id="genresFilm">
        <option value="">Tutti</option>
        <option v-for="genre in arrGenresFilm" :key="genre.id" :value="genre.id">{{ genre.name }}</option>
      </select>
      <label for="genresTv">Genere Serie Tv</label>
      <select @change="$emit('genreTvSelected', $event.target.value)" name="genresTv" id="genresTv">
        <option value="">Tutti</option>
        <option v-for="genre in arrGenresSerie" :key="genre.id" :value="genre.id">{{ genre.name }}</option>
      </select>
    </div>
    <div class="cont-input">
      <input @keyup.enter="$emit('search', strSearch), strReset()" v-model.trim="strSearch" type="text" name="search" id="search" placeholder="Cerca il Film o la serie Tv">
      <button @click="$emit('search', strSearch), strReset()">Cerca</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'HeaderBooflix',
  data () {
    return {
      strSearch:'',
    }
  },
  props: {
    arrGenresFilm: Array,
    arrGenresSerie: Array
  },
  methods: {
    strReset () {
      this.strSearch = ''
    }
  }
}
</script>

<style scoped lang="scss">
@import '../assets/styles/partial/variables';
.header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  z-index: 1;
  padding: 0 2rem;
  background-color: $bg-header;
  height: 60px;
  color: #fff;
  .cont-logo {
    flex: 0 0 150px;
    cursor: pointer;
    img {
      width: 100%;
    }
  }
  .cont-txt {
    display: flex;
    align-items: center;
    margin-left: 3rem;
    h3 {
      padding: 0 1rem;
      cursor: pointer;
      &:hover {
        color: $title-color;
      }
    }
  }
  .cont-select {
    margin-left: auto;
    select {
      margin: 0 1rem;
    }
  }
  .cont-input {
    display: flex;
    justify-content: end;
    gap: 5px;
    margin-left: 1.5rem;
    height: 30px;
    input {
      min-width: 130px;
      border: 1px solid #797979;
      border-radius: 5px;
      padding: 0 .3rem;
      color: #fff;
      background-color: transparent;
    }
    button {
      padding: 0 .6rem;
      background-color: $title-color;
      color: #fff;
      border: 1px solid #797979;
      border-radius: 5px;
      cursor: pointer;
      &:hover {
        box-shadow: inset 0 0 5px 3px rgba($color: #000, $alpha: .3);
      }
    }
  }
}
</style>
