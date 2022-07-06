<template>
  <div>
    <h2 class="mb-4">Album ID : {{$route.params.id}}</h2>
    <v-data-iterator :items="items" :items-per-page.sync="perPage" :page.sync="page" hide-default-footer @page-count="pageCount = $event" sort-by="id">
      <template v-slot:default="props">
        <v-row>
          <v-col v-for="(item,idx) in props.items" :key="idx" cols="12" md="4">
            <v-card elevation="1">
              <v-img :src="'https://picsum.photos/300/200.jpg?random='+item.id"></v-img>
              <v-card-text>{{ item.title }}</v-card-text>
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
    title: "Album -",
  },
  data: () => ({
    items:[],
    perPageItem: [12,18,24],
    perPage: 12,
    page: 1,
    pageCount: 0,
  }),
  mounted() {
    this.fetch_album()
    this.perPage = localStorage.getItem('perPage')
  },
  methods:{
    setperPage(v) {
      this.perPage = v
      localStorage.setItem('perPage', v)
    },
    async fetch_album(){
      const id = this.$route.params.id
      this.items = await this.$axios.$get('https://jsonplaceholder.typicode.com/photos?albumId='+id+'&_limit=100')
      console.log(this.items)
    },
  }
};
</script>
