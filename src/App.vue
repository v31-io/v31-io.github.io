<script setup>
import { ref } from 'vue'
import axios from 'axios'


const repos = ref([])

const fetchRepos = async () => {
  try {
    const response = await axios('https://api.github.com/orgs/v31-io/repos')
    repos.value = response.data
  } catch (error) {
    console.error(error)
  }
}

fetchRepos()
</script>

<template>
  <v-container>
    <v-card title="Hello World" class="mx-auto my-8" elevation="16" max-width="344" />
    <v-divider />
    <v-card v-for="repo in repos" :key="repo.id" class="mx-auto my-8" elevation="16" max-width="344" :title="repo.name">
      <template v-slot:prepend>
        <a :href="repo.url" target="_blank" rel="noopener">
          <v-icon color="black" icon="mdi-github"></v-icon>
        </a>
      </template>
      <template v-slot:append>
        <a :href="repo.homepage" target="_blank" rel="noopener">
          <v-icon color="black" icon="mdi-open-in-new"></v-icon>
        </a>
      </template>
      <v-card-text>
        {{ repo.description }}
      </v-card-text>
    </v-card>
  </v-container>
</template>

<style>
body {
  color: #fff;
  background-color: #232629;
}
</style>