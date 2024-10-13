<script>
export default {
  props : {
    foundedRepo : Object,
  },
  created(){
    console.log(this.foundedRepo);
  }
}
</script>

<template>
    <div class="card col-4" v-for="repo in foundedRepo" :key="repo.id">
      <img :src=repo.owner.avatar_url :alt=repo.name class="card-img-top img-fluid img-thumbnail ">
      <div class="card-body d-flex flex-column justify-content-between">
        <p class="card-link"><a :href=repo.owner.html_url> Link alla repo su Github</a></p>
        <p class="card-text">Repo : {{ repo.name }}</p>
        <p class="card-text">{{ repo.description }}</p>  
        <p class="card-text"><code>{{ repo.language ? repo.language : 'Linguaggio non specificato' }}</code></p>
      </div>
  </div>

  
</template>

<style>
</style>
