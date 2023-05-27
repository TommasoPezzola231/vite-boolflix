<script>
import { store } from '../data/store.js'
import axios from 'axios'

export default {
    name: "AppMain",
    data() {
        return {
            store,
            search: "",
            flags: {
                it: `https://upload.wikimedia.org/wikipedia/commons/thumb/0/03/Flag_of_Italy.svg/800px-Flag_of_Italy.svg.png`,
                en: `https://upload.wikimedia.org/wikipedia/commons/a/a5/Flag_of_the_United_Kingdom_%281-2%29.svg`,
                es: `https://upload.wikimedia.org/wikipedia/commons/9/9a/Flag_of_Spain.svg`,
            }
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
        takeAPIimg(element) {
            let indirizzo = `https://www.engineeringstandards.in/image/cache/data/images%20%20not%20X-180x250.jpg`

            if (element != undefined) {
                indirizzo = `${this.store.imgAPI}${element}`
                return indirizzo
            }
            return indirizzo
        },
        getFlag(element) {
            //Nullish coalescing operator

            return this.flags[element] ?? `https://upload.wikimedia.org/wikipedia/commons/6/6a/Earth_Day_Flag.png`;

            //  {
            //      let flag = this.flags[element];
            //      if (flag == undefined) {
            //          return `https://upload.wikimedia.org/wikipedia/commons/6/6a/Earth_Day_Flag.png`;
            //      } else {
            //          return flag;
            //      }
            //  }
        },
        getStar(number) {
            console.log(number)
            let starNumber = Math.ceil(number / 2)
            console.log(starNumber)
            return starNumber
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
                        <div class="card flex">
                            <img :src="takeAPIimg(film.poster_path)">
                            <h3>Titolo originale = {{ film.original_title }}</h3>
                            <h4>Titolo = {{ film.title }}</h4>
                            <p>Lingua = <img :src="getFlag(film.original_language)"></p>
                            <h6>Voto =
                                <span v-for="star in getStar(film.vote_average)">

                                    <i class="fa-solid fa-star"></i>

                                </span>
                            </h6>
                        </div>
                    </template>

                </div>

                <h2>Elenco Serie TV</h2>
                <div class="flex">

                    <template v-for="serie in this.store.serieTV">
                        <div class="card flex">
                            <img :src="takeAPIimg(serie.poster_path)">
                            <h3>Titolo originale = {{ serie.original_name }}</h3>
                            <h4>Titolo = {{ serie.namme }}</h4>
                            <p>Lingua = <img :src="getFlag(serie.original_language)"></p>
                            <h6>Voto =
                                <span v-for="star in getStar(serie.vote_average)">

                                    <i class="fa-solid fa-star"></i>

                                </span>
                            </h6>
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

    .fa-star {
        color: yellow;
    }
}
</style>