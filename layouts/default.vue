<template>
  <v-app class="font">
    <Header/>
    <v-main>
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer elevation="5" app>
      <h4 class="text-center">AdeL &copy; {{ new Date().getFullYear() }}</h4>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data () {
    return {

    }
  }
}
</script>
