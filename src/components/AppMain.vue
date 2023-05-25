<script>
import { store } from '../data/store.js'
import axios from 'axios'

export default {
    name: "AppMain",
    data() {
        return {
            store,
            search: "",
            flags: [
                {
                    it: `https://upload.wikimedia.org/wikipedia/commons/thumb/0/03/Flag_of_Italy.svg/800px-Flag_of_Italy.svg.png`,
                    en: `https://upload.wikimedia.org/wikipedia/commons/a/a5/Flag_of_the_United_Kingdom_%281-2%29.svg`,
                    es: `https://upload.wikimedia.org/wikipedia/commons/9/9a/Flag_of_Spain.svg`,

                },
            ]
        }
    },
    methods: {
        filteredFilm() {
            let filmUrl = this.takeAPIfilm(this.search)

            axios.get(filmUrl).then(result => {

                this.store.films = result.data.results
            })

            let serieUrl = this.takeAPIserie(this.search)

            axios.get(serieUrl).then(result => {
                console.log(result.data.results)

                this.store.serieTV = result.data.results
            })
        },
        takeAPIfilm(element) {
            let indirizzo = `${this.store.urlAPI}/search/movie?api_key=${this.store.APIkey}&query=${element}`

            return indirizzo
        },
        takeAPIserie(element) {
            let indirizzo = `${this.store.urlAPI}/search/tv?api_key=${this.store.APIkey}&query=${element}`

            console.log("Console log serieTV", this.store.serieTV)
            return indirizzo
        },
        getFlag(element) {
            let flag = this.flags.element
            if (flag == undefined) {
                return `https://upload.wikimedia.org/wikipedia/commons/6/6a/Earth_Day_Flag.png`
            } else {
                return flag
            }
        }
    }
}
</script>

<template>
    <main>
        <section>
            <div class="container">

                <div class="search">
                    <input @keyup.enter="filteredFilm()" type="text" v-model="search" placeholder="Cosa vuoi vedere?">
                    <button @click="filteredFilm()">Cerca</button>
                </div>

                <h2>Elenco Film</h2>
                <div class="flex">
                    <template v-for="film in this.store.films">
                        <div class="card">
                            <h3>Titolo originale = {{ film.original_title }}</h3>
                            <h4>Titolo = {{ film.title }}</h4>
                            <p>Lingua = <img :src="getFlag(film.original_language)"></p>
                            <h6>Voto = {{ film.vote_average }}</h6>
                        </div>
                    </template>
                </div>

                <h2>Elenco Serie TV</h2>
                <div class="flex">
                    <template v-for="serie in this.store.serieTV">

                        <div class="card">
                            <h3>Titolo originale = {{ serie.original_name }}</h3>
                            <h4>Titolo = {{ serie.namme }}</h4>
                            <p>Lingua = <img :src="getFlag(serie.original_language)"></p>
                            <h6>Voto = {{ serie.vote_average }}</h6>
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

h2 {
    text-align: center;
    margin-top: 2rem;
    margin-bottom: 2rem;
}

p {
    img {
        width: 20px;
        display: inline-block;
    }
}

.card {
    width: 20%;
    padding: 1rem;
    border: 1px solid red;
    background-color: greenyellow;
}
</style>