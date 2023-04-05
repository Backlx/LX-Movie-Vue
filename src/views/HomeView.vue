<template>
  <div class="home">
    <div class="feature-card">
      <router-link to="/movies/tt3896198">
        <img src="https://cdn.sforum.vn/sforum/wp-content/uploads/2023/02/john-wick-4.jpg" alt="movie john wick 4" class="feature-img">
        <div class="detail">
          <h3>John Wick 4</h3>
          <p>Với cái giá phải trả ngày càng tăng, John Wick tham gia cuộc chiến chống lại High Table trên toàn cầu khi tìm kiếm những người chơi quyền lực nhất.</p>
        </div>
      </router-link>
      <form @submit.prevent="SearchMovies()" class="search-box">
        <input type="text" placeholder="What are you looking for ?" v-model="search" />
        <input type="submit" value="Search" />
      </form>
    </div>

    <div class="movie-list">
      <div class="movie" v-for="movie in movies" key="movie.imdbID">
        <router-link :to="'/movie/' + movie.imdbID" class="movie-link">
          <div class="product-image">
            <img :src="movie.Poster" alt="Movie Poster" />
            <div class="type">{{ movie.Type }}</div>
            <div class="detail">
              <p class="year">{{ movie.Year }}</p>
              <h3>{{ movie.Title }}</h3>
            </div>
          </div>
        </router-link>
      </div>
    </div>
  </div>
</template>

<script setup>
  import { ref } from 'vue'
  import env from '../env'

  const search = ref("");
  const movies = ref([]);

  const SearchMovies = () => {
    if(search.value != "") {
      console.log("keyword Search: ", search.value);
      fetch(`http://www.omdbapi.com/?apikey=${env.apikey}&s=${search.value}`)
      .then(response => response.json())
      .then(data => {
        movies.value = data.Search;
        search.value = "";
      })
    }
  }

</script>

<style lang="scss">
.home{
  .feature-card {
    position: relative;

    .feature-img{
    display: block;
    width: 100%;
    height: 300px;
    object-fit: cover;
    
    position: relative;
    z-index: 0;
    }

    .detail {
      position: absolute;
      left: 0;
      right: 0;
      bottom: 0;
      background-color: rgba(0, 0, 0, 0.6);
      padding: 16px;
      z-index: 1;

      h3 {
        color: #FFF;
        margin-bottom: 16px;
      }

      p {
        color: #FFF;
      }
    }
  }

  .search-box {
    display: flex;
    flex-direction: column;
    justify-content: center;
    align-items: center;
    padding: 16px;
    
    input {
      display: block;
      appearance: none;
      border: none;
      outline: none;
      background: none;

      &[type="text"] {
        width: 100%;
        color: #FFF;
        background-color: #496583;
        font-size: 20px;
        padding: 10px 16px;
        border-radius: 8px;
        margin-bottom: 15px;
        transition: 0.4ms;

        &::placeholder {
          color: #f3f3f3;
        }

        &:focus {
          box-shadow: 0px 3px 6px rgba(0, 0, 0, 0.2);
        }
      }

      &[type="submit"] {
        width: 100%;
        max-width: 300px;
        background-color: #43B883;
        padding: 16px;
        border-radius: 8px;
        color: #FFF;
        font-size: 20px;
        text-transform: uppercase;
        transform: 0.4ms;

        &:active {
          background-color: #3B8070;
        }
      }
    }
  }

  .movie-list {
    display: flex;
    flex-wrap: wrap;
    margin: 0px 8px;

    .movie {
      max-width: 50%;
      flex: 1 1 50%;
      padding: 16px 8px;

      .movie-link {
        display: flex;
        flex-direction: column;
        height: 100%;

        .product-image {
          position: relative;
          display: block;

          img {
            position: block;
            width: 100%;
            height: 275px;
            object-fit: cover;
          }

          .type {
            position: absolute;
            padding: 8px 16px;
            background-color: #42B883;
            color: #FFF;
            bottom: 16px;
            left: 0px;
            text-transform: capitalize;
          }
        }
        .detail {
          background-color: #496583;
          padding: 16px 8px;
          flex: 1 1 100%;
          border-radius: 0px 0px 8px 8px;
          .year {
            color: #AAA;
            font-size: 14px;
          }
          h3 {
            color: #FFF;
            font-weight: 600;
            font-size: 18px;
          }
        }
      }
    }
  }
}
</style>
