<script>
import { store } from '../data/store.js'
import axios from 'axios'

export default {
    name: "AppHeader",
    data() {
        return {
            store,
            search: "",
        }
    },
    methods: {

        filteredFilm() {
            let filmUrl = this.takeAPIfilm(this.search)

            axios.get(filmUrl).then(result => {

                console.log("Film--------------", result.data.results)
                this.store.films = result.data.results
            })

            let serieUrl = this.takeAPIserie(this.search)

            axios.get(serieUrl).then(result => {
                console.log("SerieTV--------------", result.data.results)

                this.store.serieTV = result.data.results
            })
        },
        takeAPIfilm(element) {
            let indirizzo = `${this.store.urlAPI}/search/movie?api_key=${this.store.APIkey}&query=${element}`

            return indirizzo
        },
        takeAPIserie(element) {
            let indirizzo = `${this.store.urlAPI}/search/tv?api_key=${this.store.APIkey}&query=${element}`

            return indirizzo
        },
    }
}
</script>

<template>
    <header>
        <div class="d-flex containerHeader">
            <div class="logo">
                <h1>BOOLFLIX</h1>
            </div>

            <div class="search">
                <input @keyup.enter="filteredFilm()" type="text" v-model="search" placeholder="Cosa vuoi vedere?">
                <button @click="filteredFilm()">Cerca</button>
            </div>

        </div>
    </header>
</template>

<style lang="scss" scoped>
header {
    background-color: black;
    padding-top: 2rem;
    padding-bottom: 1.5rem;
}

.containerHeader {
    max-width: 1400px;
    margin: 0 auto;
}

.logo>h1 {
    color: red;
}

.search>input,
button {
    padding: 0.2rem;
    font-size: 15px;
}

.d-flex {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    align-items: center;
}
</style>