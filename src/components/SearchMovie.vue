<template>
  <q-card>
    <q-card-section>
      <div class="text-h6 card">
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
import axios from 'axios';
import { API_KEY } from 'src/service/constants';

export default defineComponent({
  // name: 'ComponentName',
  setup () {
    const searchModel = ref('');


    return {
      search,
      searchModel
    }

    async function search() {
      const temp = searchModel.value.replace(/ /g, '%20');
      const result = await axios.get(`https://api.themoviedb.org/3/search/movie?api_key=${API_KEY}&query=${temp}`)
      console.log(result);
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
