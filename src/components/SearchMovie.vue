<template>
 <q-card class="card">
    <q-card-section>
      <div class="text-h6">
        Search Movie By Title
      </div>
    </q-card-section>
    <q-card-section>
      <div class="row justify-between">
        <q-input
        v-model="searchModel"
        class="col-9"
        ></q-input>
        <q-btn class="col-3 q-pa-sm" @click="search" label="Search" color="primary"/>
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
                <img :src="movie.poster_path != null ? 'https://image.tmdb.org/t/p/w500/' + movie.poster_path : ''" class="poster" alt="No Image"/>
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

    <q-dialog v-model="showEmpty">
      <q-card>
        <q-card-section>
          <div class="text-h6">Empty Search Field</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          Please enter movie title into search field.
        </q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>

  </q-card>


</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import axios, { AxiosResponse } from 'axios';
import { API_KEY } from 'src/service/constants';
import { MovieResponse, Movie } from 'src/service/models';

export default defineComponent({
  // name: 'ComponentName',
  setup () {
    const searchModel = ref('');
    const movieResponse = ref<AxiosResponse<MovieResponse>>();
    const listOfMovies = ref<Movie[]>([]);
    const listOfMovieImageLinks = ref<string[]>([]);
    const showEmpty = ref(false);

    return {
      search,
      searchModel,
      listOfMovieImageLinks,
      listOfMovies,
      showEmpty
    }

    async function search() {
      if (searchModel.value != '') {
        const temp = searchModel.value.replace(/ /g, '%20');
        movieResponse.value = await axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${API_KEY}&query=${temp}`);
        if (movieResponse.value?.data.results) {
          listOfMovies.value = movieResponse.value?.data.results;
        } else listOfMovies.value = [];

      } else showEmpty.value = true;
    }
  }
});
</script>

<style lang="sass">
.card
 width: 1500px

@media (max-width: 575.98px)
 .card
  width: 400px

@media (max-width: 767.98px)
 .card
  width: 750px

@media (max-width: 991.98px)
 .card
  width: 980px


@media (max-width: 1199.98px)
 .card
  width: 1100px

.poster
 max-height: 200px
</style>
