<script>
import { store } from '../data/store.js'
import axios from 'axios'

export default {
    name: "AppMain",
    data() {
        return {
            store,
            search: ""
        }
    },
    methods: {
        filteredFilm() {
            let url = this.takeAPI(this.search)

            axios.get(url).then(result => {
                console.log(result.data.results)

                this.store.films = result.data.results
            })
        },
        takeAPI(element) {
            let indirizzo = `${this.store.urlAPI}/search/movie?api_key=${this.store.APIkey}&query=${element}`
            this.store.film = indirizzo
            console.log("Console log this.store.film", this.store.films)
            console.log(indirizzo)
            return indirizzo

        }
    }
}
</script>

<template>
    <main>
        <section>
            <div class="container">

                <div class="search">
                    <input type="text" v-model="search" placeholder="Cosa vuoi vedere?">
                    <button @click="filteredFilm()">Cerca</button>
                </div>

                <div class="flex">
                    <template v-for="film in this.store.films">
                        <div class="card">
                            <h3>Titolo originale = {{ film.original_title }}</h3>
                            <h4>Titolo = {{ film.title }}</h4>
                            <h6>Lingua = {{ film.original_language }}; Voto = {{ film.vote_average }}</h6>
                        </div>
                    </template>
                </div>

            </div>
        </section>
    </main>
</template>

<style lang="scss" scoped>
.flex {
    display: flex;
    flex-direction: row;
    justify-content: center;
    flex-wrap: wrap;
}

.card {
    width: 20%;
    padding: 1rem;
    border: 1px solid red;
    background-color: greenyellow;
}
</style>