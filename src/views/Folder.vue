<template>
    <ion-page>
        <ion-header :translucent="true">
            <ion-toolbar>
                <ion-buttons slot="start">
                    <ion-menu-button color="primary"></ion-menu-button>
                </ion-buttons>
                <ion-title>{{ $route.params.id }}</ion-title>
            </ion-toolbar>
        </ion-header>

        <ion-content :fullscreen="true">
            <ion-refresher slot="fixed" @ionRefresh="doRefresh($event)">
                <ion-refresher-content></ion-refresher-content>
            </ion-refresher>
            <ion-header collapse="condense">
                <ion-toolbar>
                    <ion-title size="large">{{ $route.params.id }}</ion-title>
                </ion-toolbar>
            </ion-header>

            <div id="container">
                <div v-for="movie in movies" :key="movie.id">
                    <MovieCard v-bind:movie="movie"></MovieCard>
                </div>
            </div>
        </ion-content>
    </ion-page>
</template>

<script>
import {
    IonButtons,
    IonContent,
    IonHeader,
    IonMenuButton,
    IonPage,
    IonTitle,
    IonToolbar,
    IonRefresher,
    IonRefresherContent,
} from "@ionic/vue";
import MovieCard from "./MovieCard.vue";
import axios from "axios";

export default {
    name: "Folder",
    components: {
        IonButtons,
        IonContent,
        IonHeader,
        IonMenuButton,
        IonPage,
        IonTitle,
        IonToolbar,
        MovieCard,
        IonRefresher,
        IonRefresherContent,
    },
    data() {
        return {
            movies: [],
        };
    },
    methods: {
        async fetch() {
            const movies = await axios.get(
                "https://api.themoviedb.org/3/movie/now_playing?api_key=3580bf75aaa90303fa62f491cfec60b9&language=en-US&page=1"
            );
            this.movies = movies.data.results;
        },
        async doRefresh(event) {
            console.log("Begin async operation");

            const res = await this.fetch();
            console.log("Async operation has ended");
            console.log(res);
            event.target.complete();
        },
    },
    mounted() {
        this.fetch();
    },
};
</script>

<style scoped>
/* #container {
  text-align: center;
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-50%);
}

#container strong {
  font-size: 20px;
  line-height: 26px;
}

#container p {
  font-size: 16px;
  line-height: 22px;
  color: #8c8c8c;
  margin: 0;
}

#container a {
  text-decoration: none;
} */
</style>
