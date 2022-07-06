<template>
  <div>
    <h2 class="mb-4">Albums</h2>
    <v-data-iterator :items="items" :items-per-page.sync="perPage" :page.sync="page" hide-default-footer @page-count="pageCount = $event" sort-by="id">
      <template v-slot:default="props">
        <v-row>
          <v-col v-for="(item,idx) in props.items" :key="idx" cols="12" md="6">
            <v-card elevation="1" :to="'/album/'+item.id">
              <v-card-title>{{ item.title }}</v-card-title>
            </v-card>
          </v-col>
        </v-row>
      </template>

      <template v-slot:footer>
        <v-row class="mt-3 mb-2 mx-1" align="center" justify="center">
          <div style="width: 60px;" class="me-2">
            <v-select :items="perPageItem" v-model.number="perPage" @change="setperPage" hide-details dense append-icon="fa-solid fa-caret-down"></v-select>
          </div>
          <v-spacer></v-spacer>
          <span class="me-2 grey--text">page {{ page }} of {{ pageCount }}</span>
          <v-pagination v-model="page" :length="pageCount" circle :total-visible="0" style="width: 100px;" next-icon="fa-solid fa-angle-right" prev-icon="fa-solid fa-angle-left"></v-pagination>
        </v-row>
      </template>
    </v-data-iterator>
  </div>
</template>

<script>
export default {
  head: {
    title: "Albums -",
  },
  data: () => ({
    items:[],
    perPageItem: [12,18,24],
    perPage: 12,
    page: 1,
    pageCount: 0,

  }),
  mounted() {
    this.fetch_albums()
    this.perPage = localStorage.getItem('perPage')
  },
  methods:{
    setperPage(v) {
      this.perPage = v
      localStorage.setItem('perPage', v)
    },
    async fetch_albums(){
      this.items = await this.$axios.$get('https://jsonplaceholder.typicode.com/albums/')
    },
  },
}
</script>
