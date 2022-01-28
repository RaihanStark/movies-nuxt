<template>
  <div>
    <h2 class="mb-3">
      <v-select
        v-model="categorySelected"
        :items="categories"
        label="Category"
        @change="switchCategory"
      ></v-select>
    </h2>
    <v-row>
      <v-col sm="4" md="3" v-for="movie in listOfMovies" :key="movie.id"
        ><v-card elevation="2" class="movie-card"
          ><div>
            <v-img
              class="movie-card__image"
              :src="'https://image.tmdb.org/t/p/w500/' + movie.poster_path"
            ></v-img>
          </div>
          <h3 class="movie-card__title ml-3 py-3">
            {{ movie.original_title }}
          </h3></v-card
        ></v-col
      >
    </v-row>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      listOfMovies: [],
      categorySelected: 'Now playing',
      categoriesApi: {
        'Now playing': 'now_playing',
        'Popular Movies': 'popular',
        'Top Rated Movies': 'top_rated',
        'Upcoming Movies': 'upcoming',
      },
    }
  },

  computed: {
    categories() {
      return Object.keys(this.categoriesApi)
    },
  },

  created() {
    this.fetchMovies('now_playing')
  },

  methods: {
    async fetchMovies(category) {
      const { results } = await this.$axios.$get(
        `movie/${category}?api_key=${process.env.TMDB_API_KEY}&language=en-US&page=1`
      )
      this.listOfMovies = results
    },

    switchCategory() {
      this.fetchMovies(this.categoriesApi[this.categorySelected])
    },
  },
}
</script>
