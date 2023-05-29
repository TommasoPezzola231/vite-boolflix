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
            let starNumber = Math.round(number / 2)
            return starNumber
        },

    }
}
</script>

<template>
    <main>
        <section>
            <div class="container">


                <h2>Elenco Film</h2>
                <div class="flex">

                    <template v-for="film in this.store.films">
                        <div class="flip-card">

                            <div class="card flex">

                                <div class="card-front">
                                    <img :src="takeAPIimg(film.poster_path)">
                                </div>

                                <div class="card-back">
                                    <h3>Titolo originale = {{ film.original_title }}</h3>
                                    <h4>Titolo = {{ film.title }}</h4>
                                    <p>Lingua = <img :src="getFlag(film.original_language)"></p>
                                    <h6>Voto =
                                        <span v-for="star in getStar(film.vote_average)">

                                            <font-awesome-icon icon="fa-solid fa-star" />

                                        </span>
                                        <span>({{ film.vote_count }})</span>
                                    </h6>
                                    <p class="descrizione">{{ film.overview }}</p>
                                </div>

                            </div>

                        </div>
                    </template>

                </div>

                <h2>Elenco Serie TV</h2>
                <div class="flex">

                    <template v-for="serie in this.store.serieTV">
                        <div class="flip-card">

                            <div class="card flex">

                                <div class="card-front">
                                    <img :src="takeAPIimg(serie.poster_path)">
                                </div>

                                <div class="card-back">
                                    <h3>Titolo originale = {{ serie.original_name }}</h3>
                                    <h4>Titolo = {{ serie.namme }}</h4>
                                    <p>Lingua = <img :src="getFlag(serie.original_language)"></p>
                                    <h6>Voto =
                                        <span v-for="star in getStar(serie.vote_average)">

                                            <font-awesome-icon icon="fa-solid fa-star" />

                                        </span>
                                        <span>({{ serie.vote_count }})</span>
                                    </h6>
                                    <p class="descrizione">{{ serie.overview }}</p>
                                </div>

                            </div>

                        </div>
                    </template>

                </div>
            </div>
        </section>
    </main>
</template>

<style lang="scss" scoped>
main {
    background-color: gray;
}

.flex {
    display: flex;
    flex-direction: row;
    justify-content: center;
    flex-wrap: wrap;
}

h2 {
    text-align: center;
    padding-top: 2rem;
    margin-bottom: 2rem;
}

p {
    img {
        width: 20px;
        display: inline-block;
    }
}

.card {
    position: relative;
    width: 100%;
    height: 100%;
    text-align: center;
    transition: transform 0.8s;
    transform-style: preserve-3d;

    .descrizione {
        margin-top: 1rem;
        padding: 0 1rem;
        font-size: 13px;
    }

    .card-front {
        display: flex;
        align-items: center;

        >img {
            width: 100%;
            object-fit: cover;
        }
    }

    .fa-star {
        color: yellow;
    }
}

/*********Animation flip card ***************/

.flip-card {
    background-color: transparent;
    width: 20%;
    height: 400px;
    border: 1px solid #f1f1f1;
    perspective: 1000px;
    display: flex;
}

.flip-card:hover .card {
    transform: rotateY(180deg);
}

.card-front,
.card-back {
    position: absolute;
    width: 100%;
    height: 100%;
    backface-visibility: hidden;
}

.card-front {
    background-color: #bbb;
    color: black;
}

.card-back {
    background-color: black;
    color: white;
    transform: rotateY(180deg);
}
</style>