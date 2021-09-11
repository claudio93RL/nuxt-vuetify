<template>
  <div>
    <h1 id="about">hola</h1>
    <v-row>
      <v-col cols="12" md="4" xs="12" v-for="item in items" :key="item.id">
        <v-card class="mx-auto my-12" max-width="374">
          <v-img height="250" :src="item.image"></v-img>

          <v-card-title>{{ item.name }}</v-card-title>
        </v-card>
      </v-col>
    </v-row>
    <div class="text-center">
      <v-pagination
        @input="cambioPagina"
        v-model="page"
        :length="pages"
      ></v-pagination>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    items: [],
    page: 1,
    pages: null,
  }),
  methods: {
    traerDatos() {
      this.$axios
        .get("https://rickandmortyapi.com/api/character")
        .then((res) => {
          this.items = res.data.results;
          this.pages = res.data.info.pages;
        });
    },
    async cambioPagina() {
      const api = await this.$axios.get(
        `https://rickandmortyapi.com/api/character?page=${this.page}`
      );
      this.items = await api.data.results;
    },
  },
  created() {
    this.traerDatos();
  },
  mounted() {
    this.cambioPagina();
  },
  updated() {
    this.$vuetify.goTo("#about");
  },
};
</script>