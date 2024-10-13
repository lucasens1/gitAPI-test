<script>
import RepoCard from '../components/RepoCard.vue';
export default{
    components : {
        RepoCard,
    },
    data(){
        return {
            items : [
                'Input di ricerca per la Repo', 
                'Bottone di ricerca funzionante', 
                'Cerca @change', 
                'Visualizzazione delle card', 
                'implementazione di un limite per pagina'
            ],
            selectedItems : [],
            repos : [],
            query : "",
            token: ""
        };      
    },
    methods : {
        async fetchDataFromLucasens1(){
            try{
                const response = await fetch(`https://api.github.com/users/lucasens1/repos`, {
                    headers: {
                        'Authorization' : `token ${this.token}`
                    }
                });
                this.repos = await response.json();
            } catch (error){
                console.error(error);
            }
        },
        async fetchDataFromG(){
            try{   
                const query = this.query;
                const r = await fetch(`https://api.github.com/search/repositories?q=${encodeURIComponent(query)}`, {
                    headers: {
                        'Authorization' : `token ${this.token}`
                    }
                });
                if(!r.ok){
                    throw new Error(`${r.status}`)
                }
                const data = await r.json();
                this.repos = data.items;
                console.log(this.repos);
            }catch(error){
                console.error('Error ', error);
            }
        }
    }
}
</script>

<template>
    <div class="p-1 border-1 border">
        <ul>
            <li v-for="(item, idx) in items" :key="idx">
                <label>
                    <input type="checkbox" v-model="selectedItems" :value="item" /> {{ item }}
                </label>
            </li>
        </ul>
        <div v-if="selectedItems.length > 0">
            <p> Completato &check; : <span v-for="select in selectedItems">{{ select }}, </span></p>
        </div>
        <div v-else>
            <p> Ora di cominciare a completare qualcosa. </p>
        </div>
    </div>

    <div class="container my-2">
        <div class="text-center d-flex align-items-center justify-content-center my-3">
            <label for="q">Cerca repo : </label>
            <input type="text" name="q" v-model="query" placeholder="cerca nome repo" id="q" class="mx-1 p-1" @input="fetchDataFromG">
            <button @click="fetchDataFromG" class="btn btn-primary">Cerca</button>
        </div>
        <div v-if="repos.length > 0" class="d-flex flex-wrap p-2 border border-1">
            <RepoCard :foundedRepo="this.repos"/>
        </div>
    </div>
</template>

<style>
</style>