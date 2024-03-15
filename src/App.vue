<template>
  <div>
    <header>
      <h1>the <strong>Movie</strong> Database</h1>
      <form action="" class="search-box" v-on:submit.prevent="handleSearch">
        <input type="search" placeholder="Search for a movie" v-model="search_query" required>
      </form>
    </header>

    <main>
      <div class="cards" v-if="movieList.length > 0">
        <Card v-for="movie in movieList" v-bind:aa="movie"/>
        <!--aa라는 이름으로 movie가 전달됨-->
      </div>
      <div class="no-result" v-else>
        <h3>'{{ search_query }}' 검색 결과가 없습니다. </h3>
      </div>
    </main>
  </div>
</template>

<script setup>
import {ref} from 'vue';
import Card from './components/Card.vue';

const movieList = ref([])
const search_query = ref('')


const handleSearch = async () => {     
  movieList.value = await fetch(`https://api.themoviedb.org/3/search/movie?query=${search_query.value}&include_adult=false&language=en-US&page=1&api_key=3258b30c99242d86ce897d5a66329077`)
  .then(response => response.json())
  .then(response => response.results)
  console.log('받아온 데이타', movieList.value)
}

// const popular = async () => {     
//   movieList.value = await fetch('https://api.themoviedb.org/3/movie/now_playing?language=en-US&page=1&api_key=3258b30c99242d86ce897d5a66329077')
//   .then(response => response.json())
//   .then(response => response.results)
//   console.log('받아온 데이타', movieList.value)
// }
// popular();


</script>

<style lang="scss" scoped>
  @import url('https://fonts.googleapis.com/css2?family=Nanum+Gothic+Coding:wght@400;700&family=Noto+Sans+KR:wght@100..900&display=swap');
  $color:#313131;


  * {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
    font-family: "Nanum Gothic Coding", monospace;
  }

  header {
    padding: 50px 0;

    h1 {
      color: #888;
      font-size: 42px;
      font-weight: 700;
      text-align: center;
      margin-bottom: 30px;
      text-transform: uppercase;

      strong {
        color: $color;
      }
    }
    .search-box {
      display: flex;
      justify-content: center;

      input {
        appearance: none;
        border: none;
        outline: none;
        background: #f3f3f3;
        padding: 15px;
        width: 100%;
        max-width: 600px;
        border-radius: 8px;
        box-shadow: 1px 2px 8px rgba(0,0,0,0.2);
        color: $color;
        font-size: 20px;
        transition: 0.3s;

        &::placeholder {
          font-size: 15px;
          color: #aaa;
        }
        &:focus {
          background: $color;
          font-size: 15px;
          color: #fff;
        }
      }
    }
  }
  
  main {
    margin: auto;

    .cards {
      display: flex;
      flex-wrap: wrap;
      max-width: 1400px;
      margin: auto;
      
    }
    .no-result {
      text-align: center;
      padding: 50px 0;
    }
  }

</style>