<script lang="ts" setup>
const movieInput = ref("");
const movies = ref();

watch(
  movieInput,
  async (newValue) => {
    if (newValue) {
      const { data } = await useFetch(
        `https://api.themoviedb.org/3/search/movie?query=${encodeURIComponent(
          newValue
        )}&include_adult=false&language=en-US&page=1`,
        {
          headers: {
            accept: "application/json",
            Authorization:
              "Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiI2ZTZmNTNjNWE2OTE5ZWM2MWE0ODYyNTQwNDJkOWI4NiIsInN1YiI6IjY1MzdiMDVlYzUwYWQyMDE0ZTIzN2E1NyIsInNjb3BlcyI6WyJhcGlfcmVhZCJdLCJ2ZXJzaW9uIjoxfQ.TRhBvGc0IfGxO7n9Po4z8VDGxlYjFPMMBQYU8OhShFg",
          },
        }
      );

      movies.value = data.value;
      console.log(movies.value.results);
      // Update the movies reactive state with the fetched data
    }
  },
  { deep: true, immediate: false }
);
</script>

<template>
  <div>
    <h1>main page</h1>
    <BFormInput v-model="movieInput" placeholder="Enter your name" />
    <p>{{ movieInput }}</p>
    <div v-if="movies">
      <div v-for="movie in movies.results" :key="movie.id">
        <p>{{ movie.overview }}</p>
        <p>{{ movie.original_title }}</p>
        <img
          :src="'https://image.tmdb.org/t/p/w500' + movie.poster_path"
          alt=""
        />
      </div>
    </div>
  </div>
</template>

<style lang="scss" scoped></style>
