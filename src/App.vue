<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>
      <router-link to="/">
        <v-btn text>
          <span class="mr-2">Home</span>
        </v-btn>
      </router-link>
      <router-link to="AddStudent">
        <v-btn text>
          <span class="mr-2">Agregar alumno</span>
        </v-btn>
      </router-link>
      <router-link to="Dashboard">
        <v-btn text class="success">
          <span class="mr-2">Panel de control</span>
        </v-btn>
      </router-link>
      
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
export default {
  name: "App",

  data: () => ({
    //
  }),
};
</script>
