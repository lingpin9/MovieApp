<template>
  <q-card class="">
    <q-card-section>
      <div class="text-h6">
        Search Movie By Title
      </div>
    </q-card-section>
    <q-card-section>
      <div class="row justify-between">
        <q-input
        v-model="searchModel"
        class="col-xs-8"
        ></q-input>
        <q-btn class="col-xs-3 q-pa-sm" @click="search" label="Search"/>
      </div>


    </q-card-section>
    <q-card-section>
      <div v-for="movie in listOfMovies" :key="movie.id" class="q-py-xs">
        <q-card v-if="$q.screen.gt.sm">
          <q-card-section horizontal>
            <q-card-section>
              <div class="column items-center">
                <img :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path" class="poster" alt="No Image"/>
              </div>

            </q-card-section>
            <q-card-section>
              <div class="text-h6">
                {{ movie.title }}
              </div>
              <q-separator />
              <div>
                Release Date: {{ movie.release_date }}

              </div>
              <q-separator />
              <div>
                <div class="text-subtitle2">Overview</div>
                {{ movie.overview }}
              </div>
            </q-card-section>

          </q-card-section>


        </q-card>
        <q-card v-else>
           <q-card-section>
            <q-card-section>
              <div class="column items-center">
                <img :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path" class="poster" alt="No Image"/>
              </div>

            </q-card-section>
            <q-card-section>
              <div class="text-h6">
                {{ movie.title }}
              </div>
              <q-separator />
              <div>
                Release Date: {{ movie.release_date }}
              </div>
              <q-separator />
              <div>
                <div class="text-subtitle2">Overview</div>
                {{ movie.overview }}

              </div>
            </q-card-section>

          </q-card-section>
        </q-card>
      </div>
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
 max-width: 1700px
 min-width: 800px

.poster
 max-height: 200px
</style>
