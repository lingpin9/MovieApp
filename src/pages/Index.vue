<template>
  <q-page class="row items-center justify-evenly">
    <search-movie/>
  </q-page>
</template>

<script lang="ts">

import { defineComponent } from 'vue';
import SearchMovie from 'components/SearchMovie.vue';

export default defineComponent({
  name: 'PageIndex',
  components: { SearchMovie },
  setup() {

    return {  };
  }
});
</script>
