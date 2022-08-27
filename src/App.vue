<template>
  <v-app>
    <v-app-bar
      app
      color="white"
      elevation="0"
    >
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://miniextensions.com/wp-content/uploads/sites/5/2019/12/placeholder.com-logo1.png"
          transition="scale-transition"
          width="160"
        />
      </div>

      <v-spacer></v-spacer>
      <div class="pa-5 mt-5">
        <v-text-field
            clearable
            label="Search"
            append-icon="mdi-magnify"

        ></v-text-field>
      </div>
      <v-btn icon>
        <v-icon color="black">mdi-account</v-icon>
      </v-btn>
      <v-btn icon>
        <v-icon color="black">mdi-cog</v-icon>
      </v-btn>
    </v-app-bar>

    <v-main>
      <HelloWorld/>
    </v-main>
  </v-app>
</template>

<script>
import HelloWorld from './components/HelloWorld';

export default {
  name: 'App',

  components: {
    HelloWorld,
  },

  data: () => ({
    //
  }),
};
</script>
