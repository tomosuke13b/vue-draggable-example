<template>
  <v-app>
    <v-main>
      <Sample />
    </v-main>
  </v-app>
</template>

<script>
import Sample from './components/Sample';

export default {
  name: 'App',

  components: {
    Sample,
  },

  data: () => ({
    //
  }),
};
</script>
