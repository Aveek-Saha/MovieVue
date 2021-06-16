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

        <ion-content :fullscreen="true" scrollEvents ref="content">
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
                    loading-text="Loading more movies..."
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
            maxPages: 1,
            endpoint: this.$route.params.id
                .toLowerCase()
                .split(" ")
                .join("_"),
            country: "US",
        };
    },
    methods: {
        async fetch(pageNumber) {
            const movies = await axios.get(
                "https://api.themoviedb.org/3/movie/" +
                    this.endpoint +
                    "?api_key=3580bf75aaa90303fa62f491cfec60b9&language=en-US&page=" +
                    pageNumber +
                    "&region=" +
                    this.country
            );
            this.movies = movies.data.results;
            this.pageNumber = movies.data.page + 1;
            this.maxPages = movies.data.total_pages;
        },
        getCountry() {
            axios
                .get("http://ip-api.com/json/?fields=countryCode")
                .then((res) => {
                    this.country = res.data.countryCode;
                    this.fetch(this.pageNumber);
                })
                .catch((err) => {
                    this.fetch(this.pageNumber);
                });
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
                "https://api.themoviedb.org/3/movie/" +
                    this.endpoint +
                    "?api_key=3580bf75aaa90303fa62f491cfec60b9&language=en-US&page=" +
                    pageNumber +
                    "&region=" +
                    this.country
            );
            this.movies = this.movies.concat(movies.data.results);
            this.pageNumber = movies.data.page + 1;
            this.maxPages = movies.data.total_pages;
        },
        async loadData(ev) {
            const res = await this.pushData(this.pageNumber);
            console.log("Loaded data");
            console.log(res);
            console.log(this.pageNumber);
            ev.target.complete();
            if (this.pageNumber >= this.maxPages) {
                ev.target.disabled = true;
            }
        },
    },
    mounted() {
        // this.fetch(this.pageNumber);
        this.getCountry();
    },
    watch: {
        $route(to, from) {
            console.log("Route changed");
            this.endpoint = this.$route.params.id
                .toLowerCase()
                .split(" ")
                .join("_");
            this.pageNumber = 1;
            this.maxPages = 1;

            this.fetch(this.pageNumber);
            console.log(this.content);
            this.scrollToTop();
        },
    },
    setup() {
        const content = ref();

        return {
            content,
            scrollToTop: () => content.value.$el.scrollToTop(),
        };
    },
};
</script>

<style scoped></style>
