<template>
    <ion-card>
        <img :src=url />
        <ion-card-header>
            <ion-card-subtitle>Card Subtitle</ion-card-subtitle>
            <ion-card-title>{{ movie.title }}</ion-card-title>
        </ion-card-header>

        <ion-card-content>
            {{ movie.overview }}
        </ion-card-content>
    </ion-card>
</template>

<script>
import {
    IonCard,
    IonCardContent,
    IonCardSubtitle,
    IonCardTitle,
    IonIcon,
    IonItem,
    IonLabel,
} from "@ionic/vue";
import { warning } from "ionicons/icons";
import { defineComponent } from "vue";

export default defineComponent({
    components: {
        IonCard,
        IonCardContent,
        IonCardSubtitle,
        IonCardTitle,
        // IonIcon,
        // IonItem,
        // IonLabel,
    },
    props: ["movie"],
    setup() {
        return { warning };
    },
    data() {
        return {
            url: "https://image.tmdb.org/t/p/original/" + this.movie.backdrop_path
        }
    },
});
</script>
