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
            <ion-infinite-scroll
                @ionInfinite="loadData($event)"
                threshold="100px"
                id="infinite-scroll"
                :disabled="isDisabled"
            >
                <ion-infinite-scroll-content
                    loading-spinner="bubbles"
                    loading-text="Loading more data..."
                >
                </ion-infinite-scroll-content>
            </ion-infinite-scroll>
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
    IonInfiniteScroll,
    IonInfiniteScrollContent,
} from "@ionic/vue";
import { ref } from "vue";
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
        IonInfiniteScroll,
        IonInfiniteScrollContent,
    },
    data() {
        return {
            movies: ref([]),
            isDisabled: ref(false),
            pageNumber: 1,
        };
    },
    methods: {
        async fetch(pageNumber) {
            const movies = await axios.get(
                "https://api.themoviedb.org/3/movie/now_playing?api_key=3580bf75aaa90303fa62f491cfec60b9&language=en-US&page=" +
                    pageNumber
            );
            this.movies = movies.data.results;
        },
        async doRefresh(event) {
            console.log("Begin async operation");

            const res = await this.fetch(1);
            console.log("Async operation has ended");
            console.log(res);
            event.target.complete();
        },
        async pushData(pageNumber) {
            const movies = await axios.get(
                "https://api.themoviedb.org/3/movie/now_playing?api_key=3580bf75aaa90303fa62f491cfec60b9&language=en-US&page=" +
                    pageNumber
            );
            this.movies = this.movies.concat(movies.data.results);
        },
        async loadData(ev) {
            const res = await this.pushData(this.pageNumber);
            this.pageNumber += 1;
            console.log("Loaded data");
            console.log(res);
            ev.target.complete();
            // if (items.value.length == 1000) {
            //     ev.target.disabled = true;
            // }
        },
    },
    mounted() {
        this.fetch(this.pageNumber);
        this.pageNumber += 1;
    },
};
</script>

<style scoped></style>
