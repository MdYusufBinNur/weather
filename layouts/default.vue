<template>
  <v-app dark>
    <Nav/>
    <NavSidebar/>

    <v-main style="background-color: #EDEFF3">
      <v-container fluid class="pa-5">
        <Nuxt/>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
import Nav from "../components/Common/Nav";
import NavSidebar from "../components/Common/NavSidebar";


export default {
  head() {
    return {
    }
  },
  name: 'default',
  components: {NavSidebar, Nav},
  middleware: ['auth'],
  data() {
    return {
      fixed: true,
      miniVariant: false,
      right: true,
      drawer: true,
      rightDrawer: false,
      fav: '/favicon.png',
      listLoader: false
    }
  },
  computed: {},
  watch: {},
  created() {

  },
  methods: {

  }
}
</script>

<style>
html {
  font-family: "Poppins", serif;
}
</style>
