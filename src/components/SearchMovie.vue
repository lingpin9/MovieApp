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

  </q-card>
</template>

<script lang="ts">
import { defineComponent, ref } from 'vue';
import axios, { AxiosResponse } from 'axios';
import { API_KEY } from 'src/service/constants';
import { MovieResponse, ImageResponse, Image } from 'src/service/models';

export default defineComponent({
  // name: 'ComponentName',
  setup () {
    const searchModel = ref('');
    const listOfMovies = ref<AxiosResponse<MovieResponse>>();
    const listOfMovieImages = ref<AxiosResponse<ImageResponse>>();
    const listOfMovieImageLinks = ref<string[]>([]);

    return {
      search,
      searchModel
    }

    async function search() {
      if (searchModel.value != '') {
        const temp = searchModel.value.replace(/ /g, '%20');
        listOfMovies.value = await axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${API_KEY}&query=${temp}`);
        console.log(listOfMovies.value?.data.results);
        await getImages();
      } else console.log('Nothing')
    }

    async function getImages() {
      // debugger;
      const movieIds: number[] = [];
      listOfMovies.value?.data.results.forEach(x => movieIds.push(x.id))
      console.log(movieIds);
      for (let i = 0; i < movieIds.length; i++) {
        console.log(movieIds[i]);
        const temp:AxiosResponse<ImageResponse> = await axios.get(`https://api.themoviedb.org/3/movie/${movieIds[i]}/images?api_key=${API_KEY}`);

        if(temp.data.posters.length > 0) {
          console.log(temp.data.posters[0])
          listOfMovieImageLinks.value.push(temp.data.posters[0].file_path);
        } else listOfMovieImageLinks.value.push('');

      }
      console.log(listOfMovieImageLinks.value);
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
