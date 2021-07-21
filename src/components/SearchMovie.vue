<template>
  <q-card class="card">
    <q-card-section>
      <div class="text-h6">
        Search Movie By Title
      </div>
    </q-card-section>
    <q-card-section>
      <q-input
        v-model="searchModel"
      ></q-input>
    </q-card-section>
    <q-card-section>
      <q-btn @click="search" label="tilted"/>
    </q-card-section>
    <q-card-section>
      <q-scroll-area style="height: 500px; max-width: 700px;">
      <div v-for="movie in listOfMovies" :key="movie.id" class="q-py-xs">
        {{ movie.title }}
        <q-card>
          <q-card-section>
            <q-img :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"/>
          </q-card-section>
        </q-card>
      </div>
      </q-scroll-area>
    </q-card-section>

  </q-card>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import axios, { AxiosResponse } from 'axios';
import { API_KEY } from 'src/service/constants';
import { MovieResponse, ImageResponse, Image, Movie } from 'src/service/models';

export default defineComponent({
  // name: 'ComponentName',
  setup () {
    const searchModel = ref('');
    const movieResponse = ref<AxiosResponse<MovieResponse>>();
    const listOfMovies = ref<Movie[]>([]);
    // const listOfMovieImages = ref<AxiosResponse<ImageResponse>>();
    const listOfMovieImageLinks = ref<string[]>([]);

    return {
      search,
      searchModel,
      listOfMovieImageLinks,
      listOfMovies
    }

    async function search() {
      if (searchModel.value != '') {
        const temp = searchModel.value.replace(/ /g, '%20');
        movieResponse.value = await axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${API_KEY}&query=${temp}`);
        if (movieResponse.value?.data.results) {
          listOfMovies.value = movieResponse.value?.data.results;
        } else listOfMovies.value = [];

        console.log(listOfMovies.value);
      } else console.log('Nothing')
    }
  }
});
</script>

<style lang="sass">
.card
 min-height: 800px
 max-width: 1500px
 min-width: 800px
</style>
